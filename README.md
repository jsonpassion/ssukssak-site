<p align="center">
  <img src="icon.png" width="88" alt="쓱싹 아이콘">
</p>

<h1 align="center">쓱싹 — 소개 사이트</h1>

<p align="center">
  <b>쓱</b> 넘기고 <b>싹</b> 비우는, 가장 산뜻한 iOS 사진 정리 앱의 랜딩 페이지<br>
  <a href="https://jsonpassion.github.io/ssukssak-site/"><b>jsonpassion.github.io/ssukssak-site</b></a>
</p>

---

## 무엇인가요

**쓱싹**은 겹친 사진·초고화질·큰 파일을 찾아 한 번에 걷어내는 iOS 사진 정리 앱입니다.
이 저장소는 그 소개 사이트로, **개인정보 처리방침**과 **이용약관**도 함께 호스팅합니다(App Store 심사 요구사항).

- 빌드 도구·프레임워크 없음 — **정적 HTML 한 장**(CSS·JS 인라인)
- `main`에 푸시하면 **GitHub Pages**가 자동 배포
- 외부 의존성은 로티 재생용 [lottie-web](https://github.com/airbnb/lottie-web) CDN 하나뿐

## 구조

```
.
├── index.html          # 랜딩 (CSS·JS 전부 인라인, 단일 파일)
├── privacy/index.html  # 개인정보 처리방침 — Data Not Collected 근거
├── terms/index.html    # 이용약관 — Apple 표준 EULA 기준
├── icon.png            # 앱 아이콘 (파비콘·로고·OG 이미지 공용)
└── assets/
    ├── lottie/         # 수제 로티 12종
    └── video/          # 시네마틱 루프 3종 + 포스터
```

## 디자인

컨셉은 앱과 동일한 **빗자루 청소** 은유 — *어수선한 밤을 걷어내고 산뜻한 아침으로*.
그래서 페이지는 **다크(히어로) → 라이트(본문) → 다크(마무리)** 로 흐릅니다.

### 브랜드 토큰

| 토큰 | 값 | 용도 |
|---|---|---|
| `--mint` | `#2BD9A2` | 핵심 강조, 그라디언트 시작 |
| `--aqua` | `#37D0C4` | 그라디언트 끝 |
| `--teal` | `#12A38A` | 본문 강조, eyebrow |
| `--deep` / `--night` | `#063F38` / `#04231F` | 다크 섹션 배경 |
| `--coral` | `#F0736B` | 할인 취소선, 삭제 신호 |
| `--bg` / `--ink` | `#F7FCFB` / `#0E211E` | 라이트 배경 / 본문 |

### 섹션

히어로 → 쓰는 법(4단계) → 기능(4종) → 미리 해보기 → 안심 설계 → **시네마틱 밴드** → 가격 → 마무리 CTA → 푸터

### 인터랙션

- **스크롤 리빌** — `IntersectionObserver`로 카드가 한쪽에서 펼쳐지듯 등장(`.reveal.from-left/right`, `--i`로 스태거)
- **시네마틱 배경 영상** — 화면 밖이면 일시정지, `prefers-reduced-motion`이면 포스터만 표시
- **나브** — 다크 히어로 위에선 투명, 본문에 닿으면 프로스트 라이트로 전환
- **미리 해보기** — 카드를 좌/우로 넘겨 담긴 용량이 집계되는 인터랙티브 데모
- **첫 방문 오버레이** — 최초 1회만(`localStorage: ssukssak_coach_v1`), 탭하면 쓱 걷힘

> 모션은 전부 `prefers-reduced-motion`을 존중합니다.

## 에셋

### 로티 12종 (`assets/lottie/`)

전부 수제 JSON이며 앱과 공유합니다. 모션 문법: **돋보기 = 찾는 중** · **빗자루질 = 실제 삭제** · **반짝임 = 완료**.

`magnify_scan` · `dust_poof` · `dustpan_fill` · `broom_sweep` (쓰는 법 4단계)
`swipe_cards` · `stack_merge` · `vacuum_shrink` · `gauge_free` (기능 4종)
`sparkle_clean` (데모 완료) · `shield_check` (안심 설계) · `broom_hello` · `arrow_bounce` (첫 방문 오버레이)

### 시네마틱 영상 3종 (`assets/video/`)

브랜드 모티프(사진 타일이 민트-아쿠아 빛에 쓸려나가는 장면)를 담은 무음 루프입니다.
AI로 생성한 뒤 웹용으로 재인코딩했습니다 — 각 340KB 미만, 720p, 포스터 `.jpg` 동봉.

| 파일 | 쓰임 |
|---|---|
| `hero.mp4` | 히어로 배경 — 흩어진 사진 타일을 빛의 물결이 쓸어냄 |
| `sweep.mp4` | 시네마틱 밴드 배경 — 고요히 깔린 사진 타일 |
| `cta.mp4` | 마무리 CTA 배경 — 안개가 걷히며 밝아오는 새벽빛 |

재인코딩 기준:

```bash
ffmpeg -i in.mp4 -an -c:v libx264 -crf 30 -preset slow \
  -movflags +faststart -pix_fmt yuv420p -vf scale=1280:720 out.mp4
ffmpeg -i out.mp4 -vframes 1 -q:v 3 poster.jpg
```

## 로컬에서 보기

`file://`로 열면 영상·로티가 제대로 로드되지 않으니 정적 서버로 띄우세요.

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

첫 방문 오버레이를 다시 보려면 콘솔에서:

```js
localStorage.removeItem('ssukssak_coach_v1'); location.reload();
```

## 배포

`main` 브랜치에 푸시하면 GitHub Pages가 루트를 그대로 서빙합니다. 별도 빌드·액션 없음.

## 법적 페이지

`privacy/`와 `terms/`는 App Store 심사 제출용이며 앱 내 페이월·설정 화면에서 직접 링크합니다.
**문구를 바꿀 때는 App Store Connect의 URL과 앱 내 링크가 여전히 유효한지 확인하세요.**

## 푸터 규격

모든 ForgeLab 앱 사이트 공통으로 5요소를 유지합니다 —
개인정보 처리방침 · 이용약관 · ✉️ 문의하기 버튼 · © 2026 ForgeLab · ForgeLab · 대표 Jason Lee.

> 문의 이메일은 **버튼 뒤 `mailto:`로만** 두고 본문에 텍스트로 노출하지 않습니다.

---

© 2026 ForgeLab · 대표 Jason Lee. 사이트 코드와 에셋의 무단 사용을 금합니다.
