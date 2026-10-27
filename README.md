# 소프트 다이소 (softdaiso) — 무료 소프트웨어 공식 다운로드 안내

`https://soft.pinda.kr` 정적 사이트 빌더. 소프트픽(softpick.co.kr)의 분류 구조를 참고해 **분류 › 항목 › 프로그램 › 공식 다운로드** 로 총망라하고, **PC 필수 프로그램** 설치 순서 체크리스트(`/pc/`)를 더했다.

- **설치 파일을 직접 올리지 않는다.** 모든 버튼은 개발사 공식 페이지로 연결한다(재배포 라이선스·악성코드 위험·광고 정책).
- 다발 1개(`bundles/main.json`) = 사이트 1개 = 워커 1개(`softdaiso-main`).
- rsuri/soft 빌더에서 분기. 다른 점: 분류(`group`) 홈, `/pc/` 체크리스트(localStorage), 홈 검색, `/all/` 전체 목록, 다이소 레드 테마.

## 페이지 구조

| 주소 | 내용 |
|---|---|
| `/` | 검색 · PC 필수 바로가기 · 분류별 항목 카드 · 운영체제 |
| `/pc/` | PC 필수 프로그램 설치 순서(단계별) — 체크박스 진행률·인쇄, 항목마다 공식 다운로드 버튼 |
| `/<항목>/` | 항목 설명 · 운영체제 탭 · 프로그램 카드 · 고를 때 볼 것 · FAQ |
| `/<항목>/<os>/` | 그 운영체제 프로그램만(전체와 같으면 생성 안 함) |
| `/os/<os>/`, `/all/` | 운영체제별 · 전체 목록(페이지 나눔) |
| `/p/<프로그램>/` | 운영체제별 공식 다운로드 버튼 · 라이선스 · 확인일 · 같은 항목 |
| `/about/` | 운영 원칙 |

## 명령

```bash
node scripts/build.mjs            # 빌드 → dist/main/
node scripts/build.mjs --check    # 검증만
node scripts/check-links.mjs      # 공식 주소 생존 확인(네트워크)
npx wrangler deploy -c wrangler.main.generated.jsonc   # 로컬 배포(wrangler login 필요)
```

main 푸시 시 GitHub Actions 가 배포한다. 저장소 시크릿 `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` 가 필요하다(없으면 빌드만 되고 배포 단계는 실패).

## 다발 필드

필수: `id` `host` `name` `title` `tagline` `checked`(YYYY-MM-DD) `contact` `keywords[1~40]` `programs`.
키워드: `slug` `keyword` `title` `intro` `group` `programs[]` (+ `pick[]` `faq[{q,a}]`).
프로그램: `name` `vendor` `license` `summary` `official` `os{windows|macos|linux|android|ios: https}` (+ `en` `features[]` `note` `builtin` `checked`).
PC 필수: `pc{title,intro,steps[{title,why,programs[]}],tips[]}`.

## 지켜야 할 것

- 공식 배포처를 **직접 열어 확인한 주소만** 넣고 `checked` 를 갱신한다. 확인 못 하면 싣지 않는다(예: 팟플레이어 — 공식 주소 미확인, 포토스케이프 X — 공식 사이트가 http 전용이라 제외).
- 라이선스는 확인한 범위만 적는다. 버전 번호·가격은 적지 않는다.
- `noindex` 는 다발에서 바꾼다(현재 `false` = 검색 노출).
