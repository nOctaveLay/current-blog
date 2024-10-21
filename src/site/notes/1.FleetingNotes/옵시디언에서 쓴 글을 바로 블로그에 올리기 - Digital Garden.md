---
{"dg-publish":true,"permalink":"/1-fleeting-notes/digital-garden/"}
---

# 왜 사용하면 좋을까요?

markdown format에 익숙하며, Obsidian을 자주 쓰는 사용자
-> blog에 대해 따로 신경 쓰고 싶지 않을 때

# 어떻게 사용하나요?

Digital Garden은 Vercel을 기반으로 한 github blog를 사용하고 있습니다. 
따라서 Github 계정과 Vercel 계정이 필요합니다.

만약 없으시다면 미리 회원 가입을 해주세요!
- [Github 계정](https://github.com/)
- [Vercel 계정](https://vercel.com/signup)

그 외 내용은 [이 글](https://dg-docs.ole.dev/getting-started/01-getting-started/)을 번역했습니다! 

1. 옵시디언에서  커뮤니티 플러그인을 사용해 [Digital Garden](obsidian://show-plugin?id=digitalgarden) 을 다운로드하고 설치합니다. **반드시 활성화를 눌러주세요!**
---
2. [GitHub - oleeskild/digitalgarden](https://github.com/oleeskild/digitalgarden) 로 들어가 "Deploy" 버튼을 눌러주세요!![[Snipaste_2024-10-21_12-17-49.PNG]]이 과정은 Vercel을 열어 Digital Obsidian Garden 레포의 복제본을 만드는 것입니다. Private로 해도 전혀 문제 없으며, Vercel에서 진행 되는 과정을 따라주세요!

>[!Tips] Create Git Repository에서 Git Scope가 잡히지 않을 때
> - \+ Add Git Account 를 눌러 추가해주세요!
> - 위의 동작을 실행하면, Repository Access가 뜨는데 All repository(모든 레포 권한) / Only selected repositories(내가 설정한 레포에만)에 큰 차이는 없습니다.
> - 이 동작은 github의 personal setting > Integrations에서 확인할 수 있습니다.

---
3. Github token을 생성해주세요! 이 token은 Github에서 passward와 같은 역할을 합니다. 플러그인이 Github 레포지터리에 새로운 노트를 만들기 위해선 이 token이 필요합니다. Github에 가입하신 후 [Token](https://github.com/settings/tokens/new?scopes=repo) 을 만드는 곳으로 가서 "Generate Token"을 누르고 Token을 생성해주세요. 권한은 repo만 주셔도 충분합니다!

>[!Note] Token을 생성한 뒤, 반드시 어디에 저장해두세요!
>Token 생성 후 그 페이지를 벗어나면 Token의 정보는 어떤 경로로도 다시 볼 수 없습니다

>[!Note] Github는 기본적으로 토큰이 유지될 기간을 정하고 있습니다.
>기간마다 토큰을 다시 만들고 싶지 않다면, "No expiration" 옵션으로 다시 만들어 주세요
 
--- 
4. 옵시디언을 열고, Digital Garden setting에 들어가세요. Github repo name, Github Username, Github token을 채워주시면 됩니다!
![[digital-garden-setting-2.PNG]]

---
5. 블로그에 글을 게재하기 위해선 2개의 property를 추가해야 합니다.

>[!tip] 어떻게 노트에 property를 추가할 수 있나요?
>먼저, property 기능은 Obsidian 버전 4.1부터 추가되었습니다. Obsidian의 버전이 4.1 이상인지 확인해주세요
>확인했다면, 다음과 같은 방법을 사용할 수 있습니다!
>- Ctrl(Cmd) + p를 눌러 **Add file property** 를 입력 후 사용
>- Ctrl(Cmd) + ; 를 사용
>- 파일 제목 오른쪽에 있는 3개의 점 아이콘 (More options) 을 누르거나 tab을 오른쪽 클릭하면 **Add file property** 가 보이는데, 이를 클릭.
>- 파일의 맨 처음에 `---`을 입력하기
> 

- `dg-home` 체크박스
	- homepage 역할을 합니다.
	- 모든 노트에 추가할 필요는 없고, 오직 홈페이지 역할을 할 노트 하나에만 추가합니다.
- `dg-pulish` 체크박스
	- 블로그에 기재된다고 알려주는 세팅입니다.
	- 이 세팅이 없다면, 그 글은 블로그에 기재되지 않습니다! (반대로 말하자면, 기재되는 모든 글들은 반드시 이 세팅을 갖고 있어야만 하죠)

---
6. 블로그에 글을 쓰기 위해서 `ctrl(command) + p` 를 눌러 "Digital Garden: Publish Single Note" 를 입력하세요.

>[!Tips] 블로그 글을 게시할 때 **Publication Center**를 사용할 수 있습니다.
---

7. 만약 글을 삭제하고 싶다면, 반드시 publication center에 들어가 "Delete notes from garden" 버튼을 눌러야 합니다.

>[!tip] 어떻게 publication center에 들어갈 수 있나요?
> 다음과 같은 방법을 사용할 수 있습니다!
>- Ctrl(Cmd) + p를 눌러 **Publication Center** 를 입력 후 사용합니다.
>- 왼쪽 아이콘 메뉴에 있는 새싹 표시를 누릅니다.


