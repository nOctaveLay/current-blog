---
{"dg-publish":true,"permalink":"/3-permanents-notes//v-scode/v-scode-ssh-pem/","created":"2025-01-15T18:22:01.015+09:00","updated":"2025-01-18T01:22:32.595+09:00"}
---



#vscode #ssh #pem #privatekey #password

내 컴퓨터에서 네트워크를 통해 원격 서버에 접속할 때, 이 접속을 안전하게 하기 위해 SSH 프로토콜이 필요합니다. 
일반적으로 터미널을 열고 명령어를 입력하여 SSH 프로토콜을 사용합니다. 하지만 VSCode의 'Remote Explorer' 확장 프로그램을 활용하면 SSH 연결과 터널링을 더 쉽게 사용할 수 있습니다.


SSH는 안전하게 연결하기 위해 '암호화된 연결'을 사용하고 있습니다. 암호를 풀기 위해선 password가 필요하고, 이 password는 key 파일이라고 부릅니다. Remote Explorer 의 Remotes 설정으로 들어가면  `~/.ssh/config`에서 서버에 대한 정보와 키 파일을 입력하게끔 되어 있습니다.
하지만 key 파일을 잡아줬음에도 불구하고 password를 입력받으라는 문장이 나올 때가 있습니다.

![Pasted image 20241030125955.png](/img/user/AttachedFiles/Pasted%20image%2020241030125955.png)

밑의 글에선 `Enter password for <username>@<server_ip>`가 뜨는 원인을 파악하고, 문제를 해결하고자 합니다.


Password를 입력하라는 문장이 떴다는 사실은 ssh를 연결할 때 Password가 제대로 들어가지 않았음을 의미합니다.
위의 사진에서 password를 입력하지 않고 Enter만 누르면 어떤 문제가 일어났는지 확인할 수 있습니다.

제가 경험한 문제는 다음과 같습니다.

### 1. Permissions are too open

SSH에 들어가는 key 파일은 보안을 위해 나만 사용할 수 있어야 합니다. 즉, 소유자를 제외한 그룹, 기타 사용자가 key 파일에 대한 권한이 있다면 보안상의 문제로 연결을 할 수 없게 만듭니다.
그렇다면 어떻게 하면 되나? 단순하게 파일의 권한을 바꿔주면 됩니다.

리눅스 계열의 운영체제의 경우 `chmod` 를 터미널에서 사용해 쉽게 권한을 변경할 수 있습니다.

```cmd
chmod 400 /path/to/file
```

윈도우는 `chmod` 명령어를 사용할 수 없습니다. 하지만 이와 비슷한 작업을 하는 실행 파일이 존재합니다. `icacls.exe`를 통해 바꿀 수 있습니다.
윈도우에서 주의해야 할 점은 **PowerShell과 Command Prompt** 에서 환경변수를 처리하는 방식이 다르다는 점입니다. 따라서 둘을 다르게 설명하겠습니다.

**PowerShell의 경우**

1. 먼저 파일에 있던 권한을 전부 초기화 시킵니다.
```cmd
icacls.exe /path/to/file /reset
```

2.  사용자에게 읽기 권한을 할당합니다.
```cmd
icacls.exe /path/to/file /GRANT:R "$($env:USERNAME):(R)"
```

3. 상위 폴더로부터 할당받은 권한들을 삭제합니다.
```cmd
icacls.exe /path/to/file /inheritance:r
```


**Command Prompt의 경우**

환경변수를 처리하는 방식만 다르기 때문에 PowerShell의 2번만 달라지고 나머지는 같습니다.

1. 파일에 있던 권한을 전부 초기화 시킵니다.
```
icacls.exe /path/to/file /reset 
```

2. 사용자에게 읽기 권한을 할당합니다.
```
icacls.exe /path/to/file /grant:r %username%:(R) 
```

3. 상위 폴더로부터 할당받은 권한들을 삭제합니다.
```
icacls.exe /path/to/file /inheritance:r
```

### 2. ssh-agent unable to start ssh-agent service, error :1058

SSH에 제대로 키가 등록되었는지 확인하기 위해선 ssh-agent를 활성화 시켜야합니다.
이 과정에서 ssh-agent가 활성화되지 않았고, 따라서 오류를 발생시킬 때 해줘야 하는 것들입니다.

### SSH 에이전트 활성화 방법

**Linux의 경우**

```cmd
eval "$(ssh-agent -s)"
```

**Window의 경우**
1. **서비스 관리 도구 열기**: Windows 검색에서 "서비스"를 검색하여 "서비스" 관리 도구를 엽니다.
2. **OpenSSH Authentication Agent 찾기**:
    - 서비스 목록에서 "OpenSSH Authentication Agent"를 찾습니다. 이 서비스는 SSH 키를 관리하는 역할을 합니다.
    - ![Pasted image 20241030130645.png](/img/user/AttachedFiles/Pasted%20image%2020241030130645.png)
    - 마우스 오른쪽 키 -> 속성 -> 시작 유형을 자동으로 변경
     ![[openssh.PNG]]
	- 밖으로 나와 마우스 오른쪽키 -> 시작

3. **이제 명령 프롬프트로 돌아가 `ssh-agent`를 다시 시도합니다.**
	- Window Powershell : `Get-Service ssh-agent` 

### 3. 모두 실행해 보았음에도 password 입력창이 뜨는 경우

- ssh key를 수동으로 등록해줍니다.
```
`ssh-add /path/to/your/key.pem`
```

### 추가 내용

- [ssh "permissions are too open" - Stack Overflow](https://stackoverflow.com/questions/9270734/ssh-permissions-are-too-open)
- [VS Code를 사용한 파이썬 SSH 원격 개발 환경 설정 및 pem 파일 권한 관련 오류 해결 방법 - Technology & Finance](https://technfin.tistory.com/entry/VS-Code%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-%ED%8C%8C%EC%9D%B4%EC%8D%AC-SSH-%EC%9B%90%EA%B2%A9-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95-%EB%B0%8F-pem-%ED%8C%8C%EC%9D%BC-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A0%A8-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95?category=867922)
