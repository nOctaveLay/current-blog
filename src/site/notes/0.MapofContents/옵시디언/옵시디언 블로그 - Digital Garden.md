---
{"dg-publish":true,"permalink":"/0-mapof-contents//digital-garden/","created":"2024-10-21T12:06:34.122+09:00","updated":"2024-11-05T10:51:09.396+09:00"}
---

---
## 왜 사용하면 좋을까요?

markdown format에 익숙하며, Obsidian을 자주 쓰는 사용자
-> blog에 대해 따로 신경 쓰고 싶지 않을 때

![[Snipaste_2024-10-21_12-17-49.PNG]]
![[digital-garden-setting-2.PNG]]


---
## 어떻게 사용하나요?

Digital Garden은 Vercel을 기반으로 한 github blog를 사용하고 있습니다. 
따라서 Github 계정과 Vercel 계정이 필요합니다.

만약 없으시다면 미리 회원 가입을 해주세요!
- [Github 계정](https://github.com/)
- [Vercel 계정](https://vercel.com/signup)

그 외 내용은 [이 글](https://dg-docs.ole.dev/getting-started/01-getting-started/)을 번역했습니다! 

1. 옵시디언에서  커뮤니티 플러그인을 사용해 [Digital Garden](obsidian://show-plugin?id=digitalgarden) 을 다운로드하고 설치합니다. **반드시 활성화를 눌러주세요!**
---
2. [GitHub - oleeskild/digitalgarden](https://github.com/oleeskild/digitalgarden) 로 들어가 "Deploy" 버튼을 눌러주세요!!

![[Snipaste_2024-10-21_12-17-49.PNG]]


이 과정은 Vercel을 열어 Digital Obsidian Garden 레포의 복제본을 만드는 것입니다. Private로 해도 전혀 문제 없으며, Vercel에서 진행 되는 과정을 따라주세요!


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


## 알면 좋은 명령어들 

>[!Notes] ctrl(command)+P 로 쓸 수 있는 명령어들은 전부 단축키가 가능하다.
>

### Quick Publish And Share Notes

현재 노트에 게시 플래그를 설정하고, 노트를 게시하며, 블로그 URL을 클립보드에 복사합니다. `Add Publish Flag -> Publish Single Note -> Copy Garden URL`과 동일하게 작동합니다. 

### Open Publication Center

블로그에 들어가거나 들어갈 예정이 있는 파일들을 전부 관리합니다.
또한, 큰 update를 할 수 있는 옵션이 있습니다.

위의 두 옵션은 단축키를 설정하는 것이 좋습니다. 더 자세한 내용은 [여기](https://dg-docs.ole.dev/getting-started/02-commands/)에서 확인하세요!

## 블로그 세팅하기

처음 블로그를 만들면 Homepage밖에 없는 화면이 기다리고 있습니다. 내가 올린 게시글을 잘 보여주기 위해서, 반드시 블로그를 세팅할 필요가 있습니다. (최소한 어떤 파일이 있는지 보여주는 정도는 세팅해야 합니다.)
이에 관련된 모든 세팅은 "Note Settings" 에 존재합니다. 

![Blog Setting.png](/img/user/AttachedFiles/Blog%20Setting.png)

Manage note settings를 누르면 다음과 같은 창이 나타납니다.

![Blog Setting2.png](/img/user/AttachedFiles/Blog%20Setting2.png)

토글을 클릭하면 클릭하는 즉시 홈페이지에 반영됩니다. 이 세팅은 전체적인 블로그 세팅으로, 만약 특정 노트에 대해 일부 기능을 활성화하거나 비 활성화하려면 front matter(전반적인 관리)를 통해 이 작업을 수행할 수 있습니다.  frontmatter/properties에 모든 세팅이 저장 되어 있습니다. 
왼쪽에 있는 파일 시스템을 부르고 싶다면, "Show filetree sidebar" 를 클릭하면 됩니다.
나머지 설정들은 적당히 설정해주시면 됩니다. 자세한 설정은 [여기](https://dg-docs.ole.dev/getting-started/03-note-settings/)를 참고하세요

## 블로그 테마 세팅

![blog_theme_setting.png](/img/user/AttachedFiles/blog_theme_setting.png)

누르면 다음과 같은 창이 나타납니다.

![appearance_setting.png](/img/user/AttachedFiles/appearance_setting.png)
이 창의 내용을 하나씩 설명하겠습니다.

### Theme

기본적인 테마는 simple dark theme가 적용됩니다. 이 플러그인은 기본적인 Obsidian theme를 지원합니다.

### Base Theme

Light, Dark 두 가지 테마가 존재합니다.

### Sitename

왼쪽 위에 보이는 블로그 이름을 설정할 수 있다. 바꾼 후 반드시 "Apply setting to site" 버튼을 눌러야 합니다.

나머지 필요한 사항은 [여기](https://dg-docs.ole.dev/getting-started/04-appearance-settings/)에 정리되어 있습니다.

### Timestemps settings

표기할 시간을 알려주는 format입니다. `Jan 01, 2020 1:30 PM`이 기본값으로 설정 되어 있습니다. 24 시간을 나타내도록 바꾸려면, `MMM dd, yyyy:HH:mm` 으로 바꾸면 됩니다.
이렇게 설정하는 값들은 [luxon 문서](https://github.com/moment/luxon/blob/master/docs/formatting.md#table-of-tokens)형식을 따릅니다.

#### 문서가 생성되거나 수정된 날짜 생성

이 날짜는 2가지 형식으로 지정할 수 있습니다. 첫 번째는 노트에 front-matter value를 설정하는 방법입니다.

![timestemp.png](/img/user/AttachedFiles/timestemp.png)
키를 특정해 놓으면,  `dg-created` 와 `dg-updated` 를 자유롭게 수정할 수 있습니다.
그렇지만, 모든 문서에 이렇게 만드는 것은 굉장히 귀찮습니다. 그래서 파일이 생성되고 수정된 날짜를 자동으로 기록해주는 방법이 있습니다. **필드에 아무것도 채워 넣지 않는 방법입니다.**

![timestemp2.png](/img/user/AttachedFiles/timestemp2.png)

이렇게 하면 파일이 생성되고 수정된 날짜에 맞춰 자동으로 블로그에서 생성 날짜 / 업데이트 된 날짜를 잡아줍니다.

## 기타 설정

### Slugify Note URL

기본 값은 slugify가 적용된 값입니다.
즉, `My folder/My note.md` 형태로 옵시디언에 저장되어 있다면, URL에서는 `my-folder/my-note`의 형태로 바꿔줍니다. 
**영어 사용자가 아닌 경우** 이 옵션을 끄지 않는다면 URL에 빈 문자열만 있는 것을 볼 수 있습니다. 제목을 한국어로 하는 경우가 많다면 반드시 꺼주도록 합니다!
**참고** 최신 버전의 옵시디언에선 영어 사용자가 아닌 경우 이 옵션이 아예 없는 것을 확인할 수 있습니다. 정상이며, 기본 값으로 slugify가 꺼져 있는 상태가 됩니다.

### 경로 다시 쓰기

만약 옵시디언에 있는 파일의 경로와 블로그에 나타난 파일의 명칭을 다르게 하고 싶다면, 이 방법을 사용하면 됩니다.

![path_rewrite_rule.png](/img/user/AttachedFiles/path_rewrite_rule.png)

위에 있는 버튼을 누르면, 다음과 같은 화면으로 전환합니다.

![path_rewrite_rule2.png](/img/user/AttachedFiles/path_rewrite_rule2.png)

이 룰을 사용할 때의 규칙은 다음과 같습니다.
1. 한 줄 당 하나의 규칙만
2. \[옵시디언 내의 주소\] :\[블로그에서 보일 폴더 주소\]
3. 첫 번째 일치에서 매칭 종료

