# 게시판모아(BoardMoa) 앱 연동 — 그누보드 모듈

스마트폰 게시판 앱 **게시판모아**가 그누보드5 사이트의 게시판을 읽고 쓸 수 있게 하는 모듈입니다.

- 앱 받기: [iPhone·iPad (App Store)](https://apps.apple.com/app/id6826114071) · [Android (Google Play)](https://play.google.com/store/apps/details?id=com.easyapplabs.boardmoa)
- 최신 버전: **1.0.21 (시험판)** · 내려받기: [releases/appapi-gnu-1.0.21.zip](releases/appapi-gnu-1.0.21.zip) · [latest.json](latest.json)
- 설치한 뒤에는 관리 화면(관리자 → 환경설정 → 게시판모아 앱)의 [업데이트 확인]으로 새 버전을 받습니다.
- XE 1.11·Rhymix 사이트용 모듈은 [boardmoa-appapi](https://github.com/colombo7/boardmoa-appapi), XE3 는 [boardmoa-appapi-xe3](https://github.com/colombo7/boardmoa-appapi-xe3), WordPress 는 [boardmoa-wordpress](https://github.com/colombo7/boardmoa-wordpress) 에 있습니다.


> **시험판입니다.** 그누보드 연동은 아직 시험 중입니다.

그누보드5 사이트의 게시판을 스마트폰 앱 '게시판모아'에서 보고 쓸 수 있게 하는 모듈입니다.
누가 무엇을 볼 수 있는지는 그누보드의 게시판 권한(목록·읽기·쓰기·댓글 레벨, 그룹 접근, 비밀글, 본인확인, 포인트)과 같습니다.

| 기능 | 앱에서 |
|---|---|
| 읽기 | 메뉴(메뉴설정의 모바일 메뉴), 게시판 목록·분류·검색, 글(첨부 사진·에디터 사진), 댓글·답글, 첨부 내려받기, 내용 페이지 |
| 쓰기 | 글쓰기(사진 첨부)·고치기·지우기, 댓글·답글·댓글 고치기·지우기, 추천·비추천, 스크랩 |
| 비회원 | 비밀번호로 비밀글 보기, 글·댓글 지우기. 글쓰기·고치기·댓글은 그누보드가 자동등록방지(캡챠)를 요구해 **reCAPTCHA v2** 를 쓰는 사이트만 앱에서, 기본 숫자 캡챠는 웹으로 안내 |
| 내 활동 | 내가 쓴 글·댓글, 스크랩, 쪽지 읽기·지우기 |

웹에서 하는 것: 회원가입, 회원정보 수정·탈퇴, 쪽지 보내기(그누보드가 회원에게도 캡챠를 요구합니다. reCAPTCHA v2 사이트는 앱에서).

XE 1.11·Rhymix 사이트는 [boardmoa-appapi](https://github.com/colombo7/boardmoa-appapi), XE3 는 [boardmoa-appapi-xe3](https://github.com/colombo7/boardmoa-appapi-xe3), WordPress 는 [boardmoa-wordpress](https://github.com/colombo7/boardmoa-wordpress) 를 설치합니다.

## 필요한 것
- 그누보드 5.4 이상 (5.6.43 에서 시험), PHP 7.0 이상
- 게시판모아 앱 2.0.3 이상
- 영카트(쇼핑몰)를 함께 쓰는 사이트도 게시판은 같게 됩니다.

## 설치 (처음 한 번)
그누보드에는 모듈을 올리는 화면이 없어서 FTP(또는 호스팅의 파일 관리자)로 올립니다.

1. `appapi-gnu-<버전>.zip` 을 받아 압축을 풉니다. 그누보드의 다른 플러그인(소셜 로그인 등)과 같은 꼴로 폴더가 두 개 있습니다.
   ```
   plugin/appapi/              ← 모듈 본체
   extend/appapi.extend.php    ← 관리자 메뉴에 '게시판모아 앱' 넣기 (선택)
   ```
2. 두 폴더를 그누보드가 설치된 폴더(`common.php` 가 있는 곳)에 그대로 올립니다. 이미 있는 `plugin`·`extend` 폴더에는 파일이 더해지기만 합니다.
3. 웹 서버가 `plugin/appapi` 폴더에 쓸 수 있게 권한을 맞춥니다(예: 707 또는 웹 서버 사용자 소유). 그래야 다음부터 관리 화면에서 자동 업데이트가 됩니다.
4. 최고관리자로 로그인해 **관리자 → 환경설정 → 게시판모아 앱** (또는 `https://<사이트>/plugin/appapi/admin.php`)을 엽니다. '연결 상태'와 자동 업데이트 가능 여부를 확인합니다.
5. 확인: 브라우저에서 `https://<사이트>/plugin/appapi/api.php?r=site` 를 열어 `"ok":true` 와 `"engine":"gnuboard"` 가 보이면 됩니다. 그 뒤 앱에서 사이트 주소를 추가하면 됩니다.

## 메뉴와 권한
- 앱의 메뉴는 **환경설정 → 메뉴설정**의 모바일 메뉴와 같습니다. 메뉴가 없으면 게시판 그룹과 게시판 순서로 만듭니다.
- 목록을 볼 수 없는 게시판(목록 레벨·PC 전용·본인확인)은 앱의 메뉴·새 글·링크 어디에도 나오지 않습니다.
- 그룹 접근을 쓰는 게시판은 웹과 같이 목록(제목)은 레벨대로 보이고, 글은 그룹 회원·관리자만 읽습니다.
- 글읽기·다운로드 포인트, 비밀글, 도배 방지 간격, 금지어, 알림 메일은 웹에서 글을 쓰고 읽을 때와 같습니다. 앱이 그누보드의 처리 파일(`bbs/write_update.php` 등)을 그대로 실행하기 때문입니다.
- 앱 로그인은 웹 로그인(`bbs/login_check.php`)과 같은 확인을 거치고 앱 전용 토큰을 줍니다. 비밀번호를 바꾸거나 회원이 차단·탈퇴되면 앱도 다시 로그인해야 합니다.

## 업데이트·지우기
- 업데이트: 관리자 → 환경설정 → 게시판모아 앱 의 **[업데이트 확인]** → **[업데이트]**. 공개 저장소에서 받아 전자서명을 확인하고, 지금 파일을 `data/appapi-backup/` 에 백업한 뒤 바꿉니다.
- 지우기: `plugin/appapi`, `extend/appapi.extend.php` 를 지웁니다. 앱 로그인 표 `g5_appapi_tokens`·`g5_appapi_views` 와 설정 파일 `data/appapi-settings.php` 는 남으니 필요하면 직접 지워 주세요.

## 문의
easyapplabs@gmail.com
