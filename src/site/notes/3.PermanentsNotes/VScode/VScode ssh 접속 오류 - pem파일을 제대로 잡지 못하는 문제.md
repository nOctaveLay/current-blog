---
{"dg-publish":true,"permalink":"/3-permanents-notes/v-scode/v-scode-ssh-pem/","created":"2024-10-30T12:59:22.544+09:00","updated":"2024-11-04T13:46:09.310+09:00"}
---

#vscode #VScode #ssh #pem #privatekey #수정중


## 문제

ssh의 private key에 해당하는 pem파일이 존재합니다.
그리고 `.ssh/config`에서도 pem파일을 잡아줬습니다.
그럼에도 불구하고 password를 찾으라는 문제가 발견했습니다.

![Pasted image 20241030125955.png](/img/user/AttachedFiles/Pasted%20image%2020241030125955.png)

밑의 글에선 `Enter password for username@server_ip`가 뜨는 원인을 파악하고, 문제를 해결하고자 합니다.

## 추정되는 원인 1 : 내가 제대로 설정했는지 다시 한 번 확인하기

VScode의 Extentions 중 하나인 Remote SSH를 사용해 SSH 서버에 접속하고자 했습니다. 이를 이용해 ssh로 접속하려면 반드시 **SSH 설정 파일**을 작성해주어야 합니다.

1. **SSH 설정 파일 확인** 

`~/.ssh/config` 파일을 열어 PEM 파일 경로와 사용자 이름이 제대로 설정되어 있는지 확인합니다. 설정 예시는 다음과 같습니다

```
Host my-server
         HostName ip_address
         User root     
         IdentityFile /path/to/your/key.pem
```

위의 설정파일의 경우, my-server라는 서버를 만드는 과정입니다.
다음과 같은 옵션이 있습니다.

| Option       | Description                                                        |
| ------------ | ------------------------------------------------------------------ |
| Host         | host 기기를 위해 기억하기 쉬운 별칭 *ex) server1*                               |
| HostName     | 서버의 HostName (대신 IP 주소를 쓸 수도 있습니다.)                                |
| User         | SSH를 통해 기기로 로그인 할 지정된 user *ex) root*                              |
| Port         | SSH로 연결할 때 사용할 port, 기본값은 22입니다. 만약 포트 번호를 지정해주고 싶다면 여기에 입력하면 됩니다. |
| IdentityFile | 당신의 private key가 저장되어 있는 파일의 위치입니다. *ex) /path/to/your/key.pem*    |
즉, private key가 잡히지 않는 문제를 해결하기 위해서 일차적으로 IdentityFile을 확인했습니다. 그럼에도 불구하고 위와 같은 문제가 계속 떴습니다.
리눅스와 맥에서는 경로 표시 형식이 윈도우와 다르기 때문에, `IdentityFile`에 설정한 경로가 정확하지 않으면 SSH 접속이 실패할 수 있습니다. 이를 확인하기 위해 현재 설정된 경로 문자열을 그대로 사용하여 해당 파일이 잡히는지 실험해 보았습니다. 

```cmd
dir <IdentityFile> /s
```

![Pasted image 20241030172526.png](/img/user/AttachedFiles/Pasted%20image%2020241030172526.png)

정상적으로 출력되는 것을 알 수 있습니다.
그렇다면 ssh에서 pem파일을 열 때 문제가 생겼을 것입니다.


2. **PEM 파일 권한 설정**  

PEM 파일 권한이 적절하지 않으면 SSH 접속 시 문제가 발생할 수 있습니다. 다음 명령어로 파일 권한을 제한하자.

`chmod 400 /path/to/your/key.pem`

하지만 window의 경우 chmod로 권한을 바꿀 수 없습니다.
따라서 다른 방법을 써야 합니다.  **cmd**에서 다음과 같은 방법으로 권한을 변경할 수 있습니다.

```cmd
icacls.exe myec2.pem /reset 
icacls.exe myec2.pem /grant:r %username%:(R) 
icacls.exe myec2.pem /inheritance:r
```

3. **VS Code Remote-SSH Extension 설정 확인**

VS Code 설정에서 `"remote.SSH.useLocalServer": false` 및 `"remote.SSH.useExecServer": true`와 같은 설정이 PEM 파일 경로와 충돌할 수 있습니다. SSH에 필요한 모든 파일을 제대로 읽을 수 있도록 이 옵션들을 변경하거나 제거해 보세요.

그럼에도 여전히 해결되지 않아, 다음과 같은 방법을 사용했다.

### 추정되는 원인2: ssh 연결이 제대로 안 되고 있나?

1. **SSH 에이전트 시작**:
    
    `eval $(ssh-agent) ssh-add /path/to/your/key.pem`
    
2. **SSH 연결 시 사용자 이름 확인**: 사용자 이름이 올바른지 확인하고, 적절한 사용자로 연결 시도:
    
    `ssh -v -i /path/to/your/key.pem -p 30797 <올바른_사용자>@10.28.224.68`
    
3. **PEM 파일 확인**: PEM 파일이 올바른지 확인하고, 다른 키 파일로 테스트해보세요.


1. 에서 다음과 같은 오류가 발생했습니다.

`C:\Users\clear>ssh-agent unable to start ssh-agent service, error :1058`

SSH 에이전트를 시작할 수 없다는 오류 1058은 서비스가 비활성화되어 있다는 것을 의미합니다. 이 문제를 해결하기 위해 SSH 에이전트를 수동으로 활성화할 수 있습니다. 다음 단계를 따라주세요:

#### SSH 에이전트 활성화 방법

1. **서비스 관리 도구 열기**:
    
    - Windows 검색에서 "서비스"를 검색하여 "서비스" 관리 도구를 엽니다.
    
2. **OpenSSH Authentication Agent 찾기**:
    
    - 서비스 목록에서 "OpenSSH Authentication Agent"를 찾습니다. 이 서비스는 SSH 키를 관리하는 역할을 합니다.
    - ![Pasted image 20241030130645.png](/img/user/AttachedFiles/Pasted%20image%2020241030130645.png)
    - ?? 왜 사용 안함으로 되어있지?
    - 마우스 오른쪽 키 -> 속성 -> 시작 유형을 자동으로 변경
    - ![[openssh.PNG]]
	- 밖으로 나와 마우스 오른쪽키 -> 시작

3. **SSH 에이전트 실행**:
    
    - 이제 명령 프롬프트로 돌아가 `ssh-agent`를 다시 시도합니다.

    `ssh-agent`

 4. **PEM 파일 권한 설정**  

PEM 파일 권한이 적절하지 않으면 SSH 접속 시 문제가 발생할 수 있습니다. 다음 명령어로 파일 권한을 제한하자.

`chmod 400 /path/to/your/key.pem`

하지만 window의 경우 chmod로 권한을 바꿀 수 없습니다.
따라서 다른 방법을 써야 합니다.  **cmd**에서 다음과 같은 방법으로 권한을 변경할 수 있습니다.

```cmd
icacls.exe myec2.pem /reset 
icacls.exe myec2.pem /grant:r %username%:(R) 
icacls.exe myec2.pem /inheritance:r
```


5. **SSH 키 추가**:
    
    - SSH 에이전트가 정상적으로 실행되면, 다음 명령어로 PEM 파일을 추가합니다:
    `ssh-add /path/to/your/key.pem`
    
