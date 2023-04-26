# <img src="https://user-images.githubusercontent.com/2356749/234554374-6977623b-880c-4b65-aec1-3841e280c4b7.svg" style="height: 32px;"> 카카오톡 CI</div>

<div align="center">
<p>
<img src="https://user-images.githubusercontent.com/2356749/234552958-94977a16-3ac5-4b02-9c6f-2fc9ab3255d6.png">
</p>
</div>

Github Action을 이용한 카카오톡 CI 봇

`function_name`에는 `send_to_me_custom` (나한테 보내기)나 `send_to_groups_custom` (친구들에게 보내기)를 사용할 수 있습니다. (템플릿 이용한 버전)

! `send_to_groups` 에서는 with: 에 receiver_uuids를 ["uuid1", "uuid2"] 형식으로 지정해 넘겨야 합니다.


## Usage

```yml
name: Example CI

on:
  push:
    branches:
      - main

jobs:
  build-and-notify:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Send KakaoTalk notification
        uses: Alfex4936/kakaotalk-ci-action@main
        with:
          kakao_access_token: ${{ secrets.KAKAO_ACCESS_TOKEN }}
          function_name: send_to_me_custom
          template_id: "93112"
          template_args: '{"title": "제목", "desc": "설명"}'
```

## Setup

1. 카카오톡 채널 앱을 만든다. [@링크](https://developers.kakao.com/console/app)

2. 카카오톡 로그인 메뉴를 활성화한다.

![image](https://user-images.githubusercontent.com/2356749/234551391-6c3bca1f-3f94-4b1d-9b18-60489ea6ae46.png)

3. 동의항목에서 이러한 메뉴들을 설정해준다.

> 동의 목적은 아무거나 적어도 된다.

![image1](https://user-images.githubusercontent.com/2356749/234551535-b05c7ece-747a-454f-b839-2c7b9f074924.png)
![image2](https://user-images.githubusercontent.com/2356749/234551641-04e9ba58-cb8f-47ec-84c2-1644bef8b312.png)

4. 팀원 관리

팀원 관리에서 메시지를 받을 팀원들 추가해준다. (EDITOR) 혼자 사용할 것이면 스킵

![image](https://user-images.githubusercontent.com/2356749/234551884-5064f15a-9c5e-447a-b11f-3d9b100873e9.png)

5. [옵션] `메시지 - 메시지 템플릿 바로가기`에서 메시지를 디자인한다.

6. Github Secret 값을 추가한다.

`KAKAO_ACCESS_TOKEN` 을 [@카카오 문서](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#kakaologin)를 참고해서 얻는다.

![image](https://user-images.githubusercontent.com/2356749/234553798-7ef4ef66-086e-4216-81b4-c2b3646c9b0a.png)

![image](https://user-images.githubusercontent.com/2356749/234553984-65ef7827-9999-4e2f-b878-93bcb0a3830e.png)

1. `.github/workflows` 폴더에 `ci.yml` Github Action 추가

예를 들어 아래와 같이 무조건 카카오톡으로 보낼 수 있다.

실패할 때만 보내려면 `if: always()` 를 추가하면 된다.

> `93112` template_id 는 title과 desc를 (제목과 설명)을 보낼 수 있습니다.

```yml
name: Example CI

on:
  push:
    branches:
      - main

jobs:
  build-and-notify:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Send KakaoTalk notification
        uses: Alfex4936/kakaotalk-ci-action@main
        # if: failure()
        with:
          kakao_access_token: ${{ secrets.KAKAO_ACCESS_TOKEN }}
          function_name: send_to_me_custom
          template_id: "93112"
          template_args: '{"title": "제목", "desc": "설명"}'
```

8. 결과

![image](https://user-images.githubusercontent.com/2356749/234552958-94977a16-3ac5-4b02-9c6f-2fc9ab3255d6.png)
