---
{"dg-publish":true,"permalink":"/3-permanents-notes//v-scode/v-scode-ssh/","created":"2024-10-30T12:23:45.712+09:00","updated":"2025-01-18T01:32:06.527+09:00"}
---

#vscode #VScode #ssh #원격서버 

# 목적

명령어를 쓰지 않고 원격 서버에 ssh 키로 접근할 때 씁니다.

# 세팅 방법

## 1. VScode를 열고 Extensions에 Remote-SSH를 검색해 설치해준다.

![Pasted image 20241030122702.png](/img/user/AttachedFiles/Pasted%20image%2020241030122702.png)

## 2. ctrl(command) + shift + p를 눌러 'Open SSH Configuration file ... '을 설치한다.

![Pasted image 20241030122905.png](/img/user/AttachedFiles/Pasted%20image%2020241030122905.png)

## 3. 접속하려는 원격 서버 정보를 입력한다.

```cmd
Host my-server
         HostName ip_address
         User root     
         IdentityFile /path/to/your/key.pem
```

| Option       | Description                                                                                                                 |
| ------------ | --------------------------------------------------------------------------------------------------------------------------- |
| Host         | An easy-to-remember alias for your host machine.                                                                            |
| HostName     | The hostname of server (you can use the IP address of the server).                                                          |
| User         | The user you've specified to log in to the machine via SSH.                                                                 |
| Port         | The port used to connect via SSH. The default port is 22, but if you've specified a unique port, you can configure it here. |
| IdentityFile | The file location where you've stored your private key.                                                                     |

## 4. Ctrl + shift + P 를 눌러 Host 연결 선택

이렇게 하면 원격 서버에 접속할 수 있습니다. 
일단 한 번 들어갔다면, Remote Explorer를 눌러 이전에 설정했던 설정으로 접속했던 서버에 바로 들어갈 수 있습니다.

![Pasted image 20250118012556.png](/img/user/AttachedFiles/Pasted%20image%2020250118012556.png)