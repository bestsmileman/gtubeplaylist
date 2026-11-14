# Gtube Playlist App (지튜브 플레이리스트 · 나만의 주크박스 & 노래사전)

> 2026-11-14 아주가 두 판(원본·개선판)을 가져옴 → 코더(클로드 코드)가 비교 → **개선판을 바탕으로 보완·개선·완성**하기로 아주 확정.
> Good Playlist App(노래 목록 → 한 번에 듣기 주소)과는 **결이 다른 앱**: Gtube는 **내 노래사전을 쌓고 그 안에서 재생목록을 꾸려 앱 안 플레이어로 듣는 주크박스**.

## 파일

| 파일 | 담긴 것 |
|---|---|
| `gtubeplaylist.html` | **작업본** = 개선판에서 시작. 모아 고치기는 이 파일에 |
| `원본/gtube_playlist_app.html` | 아주가 가져온 첫 판 (1,691줄, 손대지 않음) |
| `원본/gtube_playlist_app_improved.html` | 아주가 가져온 개선판 (1,740줄, 손대지 않음 — 작업본의 출발점) |

## 저장소와 작업 폴더 (2026-11-14 아주 결정)
- **GitHub 저장소 `GtubePlaylist`** 하나에 이 앱의 모든 파일을 모음. Good Playlist App(`co2oc/main`의 `tools/goodplaylist/`)과는 **당분간 서로 구분해 별개로** 다룸 — 파일·메모·Worker·기록을 섞지 않음.
- 아주 PC 작업 폴더 = **`C:\dev2\Claude Code(R)\Gtube Playlist`** = 이 저장소를 받은(clone) 폴더. 코더가 올리면 아주는 그 폴더에서 `git pull`, 아주가 PC에서 고치면 `git push` → 같은 파일을 함께 씀.
- 공개(웹 주소)는 아직 안 함 — 정할 때 GitHub Pages · 클로드페어 Pages 중에서 고름.

## 앱 구성 (개선판 기준)
- 노래사전(가수·곡명·videoId) + 재생목록(순서·즐겨찾기) + 유튜브 IFrame 플레이어.
- 3단계 찾기: ① 내 노래사전 → ② GitHub 공개 데이터셋(JSON·CSV) → ③ YouTube Data API(이용자 키, 1곡 100유닛).
- 여러 곡 한꺼번에 찾기(대기열), JSON 백업·복원(노래사전·재생목록·즐겨찾기, API 키는 빼고).
- Tailwind CDN, 저장은 localStorage(안 되면 메모리 + 백업 안내).

## 원본 → 개선판 비교 (2026-11-14 코더)
개선판은 같은 앱의 다음 판(약 50곳 고침): 가짜 샘플 ID 대부분 정리 · 연속 재생 오류에서 멈춤(오류 153 안내) · 재생 중 곡 빼기 정리 · 유튜브 주소 읽기(shorts·music·youtu.be) 정확히 · CSV 따옴표 · 불러온 자료 검사(`validateSongs`) · 백업에 재생목록·즐겨찾기 · 할당량 날짜를 태평양 시간으로 · 검색 15초 제한·임베드 가능한 영상만 · "평문 저장"으로 솔직한 문구 · 따옴표 든 곡 이름 단추 안전 처리 · 키보드 표시·화면낭독 · 저장이 막히면 메모리로. → **개선판을 바탕으로** (아주 확정).

## 다음 판 메모 (GT1부터, 새 메모는 GT4부터)
- **GT1. [샘플] 시험용 임시 노래 정리** (11-14) — 아주: 샘플 노래사전(21곡)·샘플 GitHub DB(7곡)의 영상 ID는 **앱을 만들며 시험하려고 임시로 넣은 것**. 개선판에도 `9S0T1U2V3W4`(볼빨간사춘기 "우주를 줄게") 같은 임시 ID가 남아 있음. → 완성할 때: 샘플을 실제로 열리는 공식 영상으로 바꾸고 하나씩 확인하거나, 처음 화면은 비워 두고 "샘플 넣어 보기" 단추로만(아주와 정함).
- **GT2. [키] 확보한 YouTube Data API v3 키를 Worker 안에 숨겨 쓰기** (11-14 아주 "goodplaylist와 GtubePlaylist는 별개로" → **Gtube 전용 Worker**(예: `gtubeplaylist`)를 따로 두는 쪽으로 바꿈. 같은 구글 키를 넣을지, 새 키를 만들지는 만들 때 아주와 정함 — 같은 키면 하루 10,000점을 두 앱이 나눠 씀. 아래는 처음 메모) (11-14 코더 → 아주 "YouTube Data API v3 키도 확보되어 있으니 참조해줘") — 지금 개선판은 이용자가 자기 키를 브라우저에 평문 저장하고 앱이 `googleapis.com`을 바로 부름(1곡 100유닛). 아주 키는 이미 **Worker `goodplaylist`의 비밀 값 `YT_API_KEY`**(구글 클라우드 world-top-leaders 프로젝트, YouTube Data API v3로만 제한, 11-12 연결 완료)에 들어 있음 → 키를 앱 파일에 적지 않고 **`https://goodplaylist.bestsmileman.workers.dev/search?q=&hl=ko&n=5`**를 부름(응답 `items:[{id,title,channel,official,…}]`, 같은 검색어 30일 캐시 · 한 사람 1분 30곡·하루 200곡 · 무료 하루 10,000점은 두 앱이 함께 씀). Good Playlist App의 고르기 점수(`gpScore`, GP1)도 함께 쓰는 길 검토. 허용 출처는 co2co.app·내 PC 파일(null)·localhost라 그대로 됨(다른 주소에 올리면 Worker `ALLOWED_ORIGINS`에 더함). "내 키 쓰기"는 선택으로 남길지 아주와 정함. ⚠ 키 값은 채팅·저장소 어디에도 적지 않음.
- **GT3. [마무리] 배포용 다듬기** (11-14 코더) — `cdn.tailwindcss.com`(개발용, 버전 없음) 대신 쓰는 꾸밈만 파일 안에 넣기 · UIID 각인 · 화면 아래 "Made with Aju & Claude"(세계언어올림픽 · 클로드 링크) 띠 · 이름 표기 정하기(Gtube는 YouTube와 헷갈릴 수 있어 상표 확인 — Good Playlist App 때처럼).























UIID: HOC-BESTSM55-CLAUDX789-20261114124716-KR-MD-V-6PENGC17WFS2-README
