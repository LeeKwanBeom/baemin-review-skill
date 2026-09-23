# 점검 기준선

점검일: 2026-09-20 (2회차 — 진단만. 수정은 사용자가 항목을 고른 뒤 별도 회차)
결함 7건 / 개선안(정확성) 5건 / 속도 개선안 5건 / 인용불가·미입증으로 제외 1건
직전 기준선 대비: 해결 4건(결함 1·2·3·4 — 저장소·설치본에 수정 원문 실재) + 개선안 ①② 완료 재확인, 미해결(이월) 3건(개선안 ③④⑤), 근거없음 0건, 신규 결함 7건(결함 1은 이월 개선안 ③의 승격, 결함 5는 속도 후보 S1의 문서-코드 불일치 부분)
점검 대상: 저장소 `SKILL.md` **637행(`wc -l` 기준; 마지막 줄에 개행이 없어 실제 줄 수는 638. 아래 행 번호는 실제 줄 번호 = Read 도구 번호)**, md5 `5af791ce7f405dc8e3c626a2940d6b0b` — 설치본(`/root/.claude/skills/synced/<uuid>_<uuid>/baemin-review/SKILL.md`)·사용자 폴더 `baemin-review.skill`(9/9 09:30) 안 SKILL.md 와 **셋 모두 동일** /
          실제 실행: 3매장 전부 Step 0~4-3(Step 5 미실행), 이어서 참 제육(계정B)·김치찜·곱도리(계정A 재로그인)에서 관찰·A/B 실험 /
          엑셀 백업: `$HOME/mnt/claude/backup/배민_백업_20260920_1132.xlsx` (원본 md5 `6dae739a…`, 헤더 1행·데이터 0행, 점검 후에도 불변) /
          브라우저: Claude in Chrome(크롬 확장)만 사용. 계정 A→B→A 전환·로그아웃·창 꺼내기는 사용자가 수행. 점검 모델: Claude Fable 5.1(Cowork 클라우드)
1차 커밋(진단): `audit/last-audit.md` 만. `SKILL.md`·`checklist.md` 는 손대지 않았다. 사용자 폴더에 사본 `baemin-review_last-audit_2026-09-20.md` 도 내려놓았다.
3차 커밋(수정 회차 2, 2026-09-23, 별도 세션): 검증 회차(2026-09-21) 결과 처리 — `SKILL.md` 749→762행(`_scroll` 라운드 시작 hidden 게이트·바닥 판정 가드·`lastGainY` 되감기), `checklist.md` v6, 이 파일에 "수정 기록 2" 절 + 2026-09-20 수정 기록 문구 4곳 정정. 재패키징·재업로드는 하지 않았다.
4차 커밋(기록 정정, 2026-09-23, 별도 세션): 검증 회차 2 결과 처리 — 이 파일만. 수정 기록 2의 `_bm_defs` 길이 라벨 정정(1곳), "검증 회차 2" 절 추가, "다음 점검에서 대조할 것"에 2건 추가. `SKILL.md`·`checklist.md` 무변경. 재패키징·재업로드는 사용자가 검증 회차 2 직후에 이미 했다.
2차 커밋(수정, 같은 날 같은 세션 — 사용자 지시): 사용자가 고른 결함 1~7 + 개선안 ①②③④⑤(a) + 추가 1건 + 속도 S1·S2·S3·S5·S6 을 저장소 `SKILL.md`(637→749행)에 반영, 점검표 개정안 1~13 을 `audit/checklist.md`(v5)에 반영, 이 파일에 "수정 기록" 절 추가. **재패키징·재업로드는 하지 않았다** — 사용자가 다른 세션에서 검증을 받은 뒤 직접 한다. 그 전까지 설치본은 진단 시점 버전(md5 `5af791ce…`)이다.

> 이번 회차의 두 목표(사용자 지시): ① 정확성 — 빠뜨리거나 잘못 읽는 리뷰 0건인지, ② 속도 — 같은 결과를 더 짧은 시간·더 적은 도구 호출로. 우선순위는 점검표 안전 규칙 > 이번 지시의 범위 > 정확성 > 속도. 속도 개선안은 "결과 집합이 현재 코드와 동일"이라는 실측이 있는 것만 올렸다.

## 시작 전 확인 결과

- 토큰: 지시문의 `push 토큰: (여기에 붙여넣기)` 자리가 비어 있어 첫 응답에서 요청 → 사용자가 이어서 제공. **작업이 끝나면 폐기할 것**(대화에 남음). 파일·remote 어디에도 남기지 않았다.
- 정본 대조: 저장소 = 설치본 = 사용자 폴더 `.skill` 내 SKILL.md, md5 `5af791ce…`, 637행(wc -l)/48,012 bytes. 직전 기준선의 "다음 점검에서 대조할 것 — 설치본 md5가 저장소와 같은지" → **같다. 재패키징·재업로드가 완료된 상태**(직전 기준선 12행의 "설치본은 진단 시점 버전(`499afc07…`)"은 이제 옛 기록).
- 엑셀 백업: 위 경로. Step 0 세 검사 `FOLDER_OK` / `OPENPYXL_OK` / `UNLOCKED` (PC git 2.34.1).
- 기준선: `audit/last-audit.md` 있음(2026-09-09 회차 + 정정·교차정정). 쿠팡 저장소(`coupang-review-skill`, `audit/last-audit.md` 383행, SKILL.md 495행 md5 `abeb17eb…`)도 clone 해 P7 대조에 썼다.
- 사용자 폴더 CLAUDE.md 의 baemin-review v2~v25 이력은 2026-09-06 전면 개정 이전 구조라 판정 근거로 쓰지 않았다(지시대로 "왜 그때 그랬는가" 참고만 — v7·v10의 "폴링마다 파서 호출로 느려졌다"는 S2 설계에 반영: 폴링은 싼 값만 보고 `_parse` 는 라운드당 1회).

## 직전 기준선 판정

| 항목 | 판정 | 근거(지금 원문·실측) |
|---|---|---|
| 결함 1 (hidden 전제·스크린샷 깨우기) | **해결됨** | 26행 `**탭이 \`document.hidden === true\`로 시작할 수 있다 — 스크린샷·wait로는 못 푼다**` / 126행 `\`hidden === true\` → **Step 3으로 가지 않는다.**` / 335행 `if (roundMs.length >= 3 && roundMs.slice(-3).every(ms => ms > 600)) { throttled = true; break; }` / 627~629행 감속·정지·Step 2 hidden 3행. [실측] 새 탭 `hidden: true` 로 시작, `setTimeout(250)` 1257/995/1007/996/995ms, rAF 2초 내 0회, `_scroll` 3라운드 1129/1005/990ms → **3.1초에 `throttled: true` 반환**(gained 0). 사용자가 창을 꺼낸 뒤 hidden false, 타이머 254/260/255/250/250ms, rAF 16ms |
| 결함 2 (별점 aria-label 주석) | **해결됨** | 231~235행 `// 별점 읽기 순서: aria-label → data-* → SVG 색상 → img alt.` … `즉 **현재 사이트에서 실제 경로는 색상 fallback**이다.` / 621행 트러블슈팅 행. [실측] 3매장 349건(173+86+90) parseFail 0, 이상값 0, 분포 김치찜 0/0/0/1/172 · 곱도리 0/0/0/2/84 · 참제육 0/1/0/2/87 |
| 결함 3 (Step 0 한 줄 판정) | **해결됨** | 49~51행 `[ -d "$HOME/mnt/claude" ] && echo FOLDER_OK \|\| echo NO_FOLDER` / `python3 -c "import openpyxl" 2>/dev/null && echo OPENPYXL_OK \|\| echo NO_OPENPYXL` / `[ -e "$HOME/mnt/claude/~\$배민_저점수리뷰.xlsx" ] && echo LOCKED \|\| echo UNLOCKED`. [실측] PC에서 세 줄 정상 출력 |
| 결함 4 (낡은 수치) | **해결됨** | 207행 `(2026-09-09 실측: 최상단 6개, 스크롤 중 최대 15개)` / 26·365행 `190건을 약 50초` / 221행 `381/381` / 417행 `2026-09-09 재확인`. [실측] 오늘 DOM 카드 8~13(중앙값 12), 173건 productive 50.5초(2회) — 수치 여전히 유효 |
| 개선안 ① `throttled` | **완료 재확인** | 위 결함 1 실측. 반환 시간은 3.1초(첫 라운드부터 600ms 초과) — 정정 회차 기록 "3.1~5.5초로 변한다" 와 일치. 판정 기준은 시간이 아니라 3라운드 연속 600ms 초과 |
| 개선안 ② `blocked`·`countMatch` | **완료 재확인** | 227행 `if (card.innerText?.includes('게시중단 요청으로 인해')) { window._blocked[no] = 1; continue; }` / 402행 `countMatch: ET === null ? null : (all.length + blocked === ET)`. [실측] 김치찜 173+1=174(게시중단 2026082401079865, 9/9와 동일 건), 곱도리 86+0=86, 참제육 90+0=90 → **3/3 성립**(9/9 포함 6/6) |
| 개선안 ③ 필터 적용 확인 | **미해결 → 결함 1로 승격** | 178행 `labelOk: document.body.innerText.includes(label),` 그대로. [실측] 적용 무효 조건을 만들어 오탐 재현(아래 결함 1·P1) |
| 개선안 ④ (a) DT `'픽업'` (b) `titleOk` 헤더 한정 (c) 하단 배너 | **미해결(이월)** — (c)는 결함 아님 재확인 | 214행 `const DT = ['가게배달','한집배달','알뜰배달','배달','포장','직접배달','배민배달','가게포장'];` 에 `픽업` 없음 / 106행 `titleOk: document.body.innerText.includes('<매장명>'),`. [실측] 김치찜 30일 첫 줄 분포 알뜰배달 149·한집배달 21·가게배달 2·**픽업 2**(2026082801677641, 2026082702904106) — 픽업 카드 닉네임 `윤다식` 은 3차 fallback(날짜 앞줄)으로 정상 파싱, 곱도리 픽업 0. (c) 배너 `TemporaryDeliveryRegionAutoApplyFloatingBanner-module` fixed z101 (598,715)~(1308,839) @innerHeight 855: 다이얼로그 **닫힘** 상태 기간 버튼 (473,532)~(1289,596) `elementFromPoint` 본래 요소 / **열림** 상태 다이얼로그 (700,156)~(1220,699), 적용 (720,631)~(1200,679) 본래 요소, `최근 30일` 라벨 (740,306)~(919,330) 본래 요소, 기간 버튼만 다이얼로그 SPAN 이 덮음(배너 무관). 2회 연속 겹침 없음 |
| 개선안 ⑤ 자동 백업·저장 후 재열기 | **미해결(이월)** | Step 0(44~60행)에 백업 블록 없음, Step 5 뒤 재열기 확인 없음. 쿠팡 SKILL.md 47~50행에는 동일 취지 블록이 이미 있음(P6) |
| "다음 점검에서 대조할 것" — 쿠팡 개정안 중 배민 해당분 | **판정 완료** | 아래 P7 양방향 대조표 |
| 〃 — 설치본 md5 | **해결됨** | 위 정본 대조 |
| 〃 — 검증 세션이 볼 것(hidden 게이트 대기, throttled 재현, 이어서 호출, Step 0 세 줄, 게시중단 열, countMatch) | **전부 실측 통과** | 위 결함 1·3, 개선안 ①② 행 |
| 〃 — `collected + blocked === expectedTotal` 반복 성립 → `ok` 편입 여부 | **3/3 성립. 편입 여부는 사용자 결정(개선안 ①·P2)** | 위 개선안 ② 행 |
| 〃 — 45초 CDP 타임아웃 재현·F 실험 | **미재현·F 별도 기록(아래 실행 실측 기록 F 절)** | 이번 회차 CDP 타임아웃 0회(`_scroll` 최대 25.5초) |
| 〃 — 곱도리 닉네임 실패 카드 구조 | **판정 완료 → 결함 4** | [실측] 첫 줄 `알뜰배달` → `내가주문한`(닉네임) → `2026년 8월 24일` → 리뷰번호 → `3회 주문 고객` → `(최근 6개월 누적 주문)` → `진짜 맛있어요👍👍👍` → 주문메뉴 `100% 한우 대창 곱도리탕 1인 단품 (밥X,반찬X)`. 닉네임에 `주문` 이 들어 268행 금지어에 걸림. 픽업·DT 와 무관 |
| 〃 — 로그아웃 상태 URL 패턴 | **판정 완료(실측)** | 아래 P3. 두 회차 연속 [추론]이던 항목이 [실측]으로 바뀜 |
| 〃 — `expectedTotal === null` 강제 시 흐름 | **미실측(이월)** | 이번에도 조건을 만들지 않음 — 4-3 은 394행 `else if (ET === null) { ok = false; … }` 로 코드상 명확해 우선순위 낮음 |

## 결함

행 번호는 638줄(실제 줄 수, Read 도구 번호) 기준. 심각도 순.

| # | 심각도 | 줄 | 문제 원문(그대로) | 실측/추론 | 왜 틀렸는지 | 수정 방향 |
|---|---|---|---|---|---|---|
| 1 | 중 | 178, 189, 194 | 178: `labelOk: document.body.innerText.includes(label),` / 189: `- **\`filterOk\`** — 기간 필터가 실제로 적용됐는가. \`ok && (labelOk \|\| range가 실제 30일 범위)\`이면 \`true\`.` / 194: `- \`ok && labelOk\` → \`filterOk = true\`.` | [실측] 조건 재현 | **적용 클릭이 무효여도 `filterOk` 가 true 가 된다.** 조건을 만들어 확인: reload(기본 6개월, 전체 915) → 다이얼로그 열고 `최근 30일` 라디오만 클릭, **적용은 누르지 않고** 2.8초 뒤 Step 3 반환식과 같은 계산 → `labelOk: true`(열린 다이얼로그가 모든 기간 라벨을 본문에 갖고 있어서), `range: "2026. 3. 21 (토) ~ 2026. 9. 20 (일)"`(6개월 그대로), `expectedTotal: 915`. 194행 판정대로면 `filterOk = true`·ET 915 로 Step 4 진행 → 6개월치를 "최근 30일 현황" 엑셀에 동기화하게 된다(`ok: true` 이므로 저장 단계가 막지 못함). 다이얼로그를 닫으면 labelOk 는 false 로 돌아온다 → 오탐의 필요조건은 "다이얼로그가 열린 채 남음" 이며, 실제 적용 클릭이 조용히 무효가 되는 사례는 4회 실행 × 3매장에서 0건(빈도 미확인). 그러나 안전장치 자체가 그 실패를 구조적으로 못 잡는다 | (P1에서 시험한 판정) **다이얼로그 닫힘 + 기간 버튼 자신의 텍스트에서 읽은 두 날짜가 `오늘-30일 ~ 오늘` 과 일치** 로 교체. 시험 결과: 열린 상태 `pass:false`(dialogOpen true, rangeStart 2026-03-21 ≠ 2026-08-21), 닫고 미적용 `pass:false`, 정상 적용 후 `2026-08-21 ~ 2026-09-20` 일치. 코드 원문은 아래 "정확성 판정" P1 항목 |
| 2 | 중 | 105, 109, 114~118 | 105: `JSON.stringify({ dismiss: _d, shopIdOk: location.href.includes('<SHOP_ID>'),` / 109: `url: location.href, hidden: document.hidden })` / 115: `- \`isLogin\` 또는 \`noShop\` 또는 \`titleOk === false\` → **이 매장만 중단**하고 다른 매장은 계속한다.` | [실측] 조건 재현 | **로그인이 풀린 바로 그 상황에서 Step 2 판정값이 하나도 안 보인다.** 로그아웃 상태로 매장 URL 접근 → `https://biz-member.baemin.com/login?returnUrl=https%3A%2F%2Fself.baemin.com%2Fshops%2F14697934%2Freviews&__ts=…` 로 리다이렉트. 이 페이지에서 Step 2 반환식(109행 `url: location.href` 포함)을 실행하면 브라우저 도구가 결과 전체를 **`[BLOCKED: Cookie/query string data]`** 로 바꿔 돌려준다 — `isLogin`·`shopIdOk`·`titleOk` 를 읽을 수 없다. 114~118행 판정에는 BLOCKED 결과에 대한 규칙이 없다. 덧붙여 `returnUrl` 안에 shopId 가 들어 있어 105행 `shopIdOk` 는 로그인 페이지에서도 true 가 되는 값이다(차단이 풀려도 단독 판정 근거로 부적합). 같은 이유로 이번 세션의 다른 JS 반환(`location.href` 포함)도 1회 차단됨 | 109행을 `path: location.host + location.pathname`(쿼리 제외)으로. 판정에 "결과가 `[BLOCKED:` 로 시작하면 로그인 필요로 간주하고 위 문구로 요청" 추가. `_scroll` 반환은 `isLogin` 불리언만 담아 영향 없음(실측 정상) |
| 3 | 하~중 | 215, 282, 286~292, 300~302 | 215: `const PK = ['사장님께만 보이는','파트너님에게만','파트너에게만','비공개 리뷰','점주에게만'];` / 292: `if (pl.length) { partner = pl.slice(0, 5).join(' ').trim(); break; }` / 300: `if (pub && partner) review = \`${pub} / [파트너전용] ${partner}\`;` | [실측] 9/9건 | **"파트너님에게만 보이는 리뷰" 카드의 리뷰내용이 틀리게 저장된다(누락은 아님).** 현재 사이트에서 이 문구는 별도 비공개 구간의 머리말이 아니라 **리뷰 전체가 비공개임을 알리는 라벨 한 줄** `파트너님에게만 보이는 리뷰입니다.` 이고 그 다음 줄부터 본문이다. 코드는 (a) 282행 필터로 라벨 줄만 빼고 본문을 `pub` 으로 잡은 뒤, (b) 286~292행이 키워드 뒤 텍스트(라벨 잔여 `보이는 리뷰입니다.` + 본문)를 다시 `partner` 로 잡아 → 300행에서 **본문이 두 번** 들어간다. 실측 예 참제육 2026091002657117: `오늘도 맛있었습니다. 근데 사장님 오늘 평소보다 맵던데 맵기맛 바뀐거아니죠? / [파트너전용] 보이는 리뷰입니다. 오늘도 맛있었습니다. 근데 사장님 오늘 평소보다 맵던데 맵기맛 바뀐거아니죠?`. 3매장 파트너전용 9건(김치찜 5·곱도리 3·참제육 1) 중 **7건이 이 중복 패턴**, 나머지 2건(김치찜 2026091102615435, 2026090501582754)은 카드에 `주문메뉴` 가 없어 `pub` 이 비고(279행 `if (ms > 0 && di2 >= 0)` 불충족) `[파트너전용] 보이는 리뷰입니다. 본문…` 으로 라벨 조각이 섞이며 292행의 5줄 상한에 긴 본문이 잘릴 수 있다(2026091102615435 는 본문 2줄이라 잘리진 않음). 저점수가 이 유형이면 엑셀 리뷰내용이 위와 같이 저장된다 | 라벨 줄(`/^(파트너님|사장님|점주)[^\n]*보이는 리뷰입니다\.?$/`)은 **플래그**로만 쓰고 본문에서 제외: `partnerOnly = true` 면 `review = '[파트너전용] ' + pub`(중복 없이). 기존 "별도 비공개 구간" 구조(키워드 뒤에만 텍스트)가 다시 나타날 때를 대비해 현 `partner` 경로는 `pub` 이 빈 경우의 fallback 으로만 유지. 5줄 상한(292행)은 본문이 들어오는 경로에서는 제거 |
| 4 | 하 | 268 | `const okNick = n => n && n.length < 30 && !n.startsWith('(') && !/주문\|리뷰번호\|배달리뷰\|답글\|사장님/.test(n) && !DT.includes(n);` | [실측] 1/349 | 닉네임에 금지어가 포함되면 3개 전략이 모두 같은 `okNick` 으로 기각돼 닉네임이 빈다. 곱도리 2026082400693614 의 닉네임은 **`내가주문한`** — `/주문/` 에 걸려 `nick_fail`(9/9·9/20 두 회차 재현, 리뷰 자체는 정상 수집·별점 5). 금지어는 메타 줄(`N회 주문 고객`, `리뷰번호 …`, `배달리뷰`, `사장님`)을 피하려는 것인데 부분 일치라 사용자 닉네임까지 막는다 | 부분 일치 금지어를 **메타 줄 정확 패턴**으로 좁힘: `/^\d+회\s*주문\s*고객$/`, `/^리뷰번호/`, `/^배달리뷰$/`, `/^사장님$/`, `/^\(최근/`. `n.length < 30`·DT 제외는 유지 |
| 5 | 하 | 336, 363, 370~371 | 336: `if (target && a >= target) break;` / 363: `이 한 줄을 **\`collected\`가 \`expectedTotal\`에 도달하거나 \`gained === 0\`이 연속 2회 나올 때까지** 반복 호출한다.` / 371: `\`collected + blocked\`가 \`expectedTotal\`과 같으면 수집이 완전한 것이다` | [실측] | **문서-코드 불일치.** 371행이 정의한 "완전"(collected + blocked = 전체)과 336행의 종료 판정(`a` = collected 만)이 다르다. 게시중단이 1건이라도 있으면 target 에 영원히 못 닿아 배치가 예산을 끝까지 쓰고, 363·370행 문구("gained 0 연속 2회")를 지키면 무진전 배치가 **2회(50초)** 추가된다. 오늘 김치찜(전체 174 = 173 + 게시중단 1)에서 그대로 재현: 4회 호출 94R·25.0s·+95 → 83R·25.5s·+65(여기서 이미 173+1=174 완전) → **48R·25.3s·+0 → 48R·25.3s·+0**, 합 101.1초 중 50.6초가 종료 확인. 9/9 기록의 3번째 호출 `48R·25.3s·+0` 도 동일 기전이며, 그날은 문서와 달리 **무진전 1회로 끝냈다**(문서 370행과 관행의 불일치). 결과 정확성에는 영향 없음(집합 동일) | 코드 쪽은 속도 개선안 S1(target 판정 `collected + blocked`, 바닥 도달 조기 반환). 문서 쪽은 363·370행을 S1 채택 여부에 맞춰 한 문장으로 정리(무진전 종료 기준을 "바닥 도달 반환 2회" 또는 "gained 0 2회" 중 하나로 고정) |
| 6 | 하 | 334 | `// 감속이면 예산을 다 쓰지 않고 즉시 반환한다(실측: 5.5초에 반환, 예전에는 20초를 gained 0으로 소진). 호출부가 사용자에게 창을 꺼내달라고 요청한다.` | [실측] | 상수처럼 읽히는 기록 문구. 반환 시간은 첫 라운드 콜드 스타트 유무로 변한다 — 9/9 검증 3.088초, 오늘 3.125초(1129/1005/990ms 3라운드), 9/9 수정 회차 5.5초. 정정 회차가 "다음 수정 회차에 함께 고칠 후보"로 남긴 항목 | `(실측: 3.1~5.5초에 반환 — 판정 기준은 시간이 아니라 3라운드 연속 600ms 초과)` 로 |
| 7 | 하 | 419 | `만약 반환값이 잘린 것으로 보이면(끝이 잘린 JSON) 그때만 폴백한다:` | [실측] 3회 | `javascript_tool` 반환은 약 1,000자에서 **끝에 `[TRUNCATED]` 표식이 붙어** 잘린다(이번 세션 3회 관측: P3 샘플 JSON, 곱도리 카드 덤프, step 4500 결과). "끝이 잘린 JSON" 이라는 서술은 표식이 없는 것처럼 읽혀 다른 판정 기준(눈으로 끊김 확인)을 만든다. 쿠팡 스킬은 같은 사유로 2026-09-09 결함 2·정정 2에서 `[TRUNCATED]` 기준으로 통일했다(P7 이관 대상) | `만약 반환값 끝에 \`[TRUNCATED]\` 표식이 붙어 잘렸으면 그때만 폴백한다:` 로. 트러블슈팅 표에도 같은 표식으로 |

인용불가·미입증으로 제외한 항목 1건: 4-2 375행 `최대 8회까지만 호출한다` 상한이 S2·S5 채택 후(라운드 100ms 안팎, 25초 배치에 ~250라운드) 과하게 큰 값이 되는지 — 채택 전이라 판정 보류[추론].

## 개선안 (정확성, 최대 5)

| # | 내용 | 이유 | 우선순위 |
|---|---|---|---|
| ① (P2) | `countMatch` 를 `ok` 판정에 편입: `else if (ET > 0 && all.length + blocked !== ET) { ok = false; reason = '전체와 N건 차이' }` (98% 규칙은 유지하거나 대체) | [실측] 이번 3/3 + 9/9 3/3 = **6/6 정확 일치**(174=173+1, 86, 90 / 190=189+1, 96, 96). **편입 비용:** 전체(N)에 포함되지만 렌더링되지 않는 다른 비노출 유형이 생기면 그 매장 저장이 막힌다(기존 행 보존, 저점수는 채팅에 보고되므로 데이터 유실은 없음 — 재실행 안내 1회의 비용). **편입 이득:** 지금은 98% 규칙이라 174건 중 1~3건 누락(≥171)이 `ok: true` 로 통과하는데, 편입하면 1건 누락도 잡는다. 사용자 결정 사항 | 1 |
| ② (P6) | 실행 전 자동 백업을 Step 0 에 넣기 — 쿠팡 SKILL.md 47~50행 블록 이식: `mkdir -p $HOME/mnt/claude/backup && if [ -f "$HOME/mnt/claude/배민_저점수리뷰.xlsx" ]; then cp "$HOME/mnt/claude/배민_저점수리뷰.xlsx" "$HOME/mnt/claude/backup/배민_저점수리뷰_$(date +%Y%m%d_%H%M%S).xlsx" && echo "백업 완료: …" \|\| echo "백업 실패 — 중단하고 사용자에게 알린다"; else echo "백업 대상 없음(첫 실행)"; fi` + 454행 "실행 전에 파일을 백업해 둘 것" 문구를 자동 백업 체계로 갱신 | 이월 ⑤(a). 스냅샷 동기화라 백업이 유일한 되돌리기 수단인데 절차에 없다. 수집 방식이 아니라 절차 이식이라 금지 대상 아님 — 사용자 승인 사항 | 2 |
| ③ | Step 5 저장 후 엑셀을 다시 열어 행수·리뷰번호 집합이 `kept + new` 와 같은지 확인하는 한 줄 | 이월 ⑤(b). 쿠팡 개선안 2(구 4)와 동일. 사본 dry-run 으로 검증 비용이 작다 | 3 |
| ④ | `parseFail` 비율 임계: 전건 실패(396행)만 잡는 판정에 `parseFail > all.length * 0.1` 을 `ok: false` 조건으로 추가 | 점검표 C "절반이 실패해도 통과하는 게 맞는지". 오늘 3매장 parseFail 0 이라 실효는 없었다. 쿠팡 개선안 3(구 5)와 동일 취지 | 4 |
| ⑤ | 사이트 현행화(이월 ④): (a) 214행 `DT` 에 `'픽업'` 추가 (b) 106행 `titleOk` 를 헤더 줄로 한정 | (a) [실측] 김치찜 30일에 픽업 2건. 지금은 3차 fallback 이 구하지만 1차 전략이 놓치는 유형. (b) 본문 어디에 매장명이 있어도 통과하는 약한 판정. (c) 배너는 2회 연속 겹침 없음 → **개선안에서 제외** | 5 |

개수 제한(5)으로 제외한 후보 1건: `(텍스트 없음)` 리뷰(3매장 105/349건)에서 `배달리뷰` 칩(`좋아요`/`아쉬워요`)을 `[배달리뷰] 아쉬워요` 로 보존하는 것 — 현재 296행 태그 분기는 `ms <= 0`(주문메뉴 없는 카드)에서만 돌아 `주문메뉴` 가 있는 카드의 칩은 기록되지 않는다[실측 2026091802546606: 칩 `아쉬워요`, 저장 `(텍스트 없음)`]. 정보 손실이지 오독은 아니어서 후순위.

## 속도 기준선 (이번 회차 신설 — 현재 설치본 코드 그대로, 실험 전에 측정)

조회 기간(3매장 공통): `2026. 8. 21 (금) ~ 2026. 9. 20 (일)`. 6개월 총건수 김치찜 `전체(1,435)`(쉼표 실재)·곱도리 512·참제육 915. 시각은 페이지 `Date.now()`(ms epoch) 기준.

| 매장 | expectedTotal | collected | blocked | countMatch | parseFail | `_scroll` 호출 수 | 라운드 합 | avgRoundMs | elapsedMs 합 | 벽시계 navigate→4-3 결과(초) — 사용자 대기 제외 / 포함 | 도구 호출 수(Step 2~4-3, 스킬 규정분) | 건/초 | hidden 시작값 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | 174 | 173 | 1 | true | 0 | 4 (94R·25.0s·+95 → 83R·25.5s·+65 → 48R·25.3s·+0 → 48R·25.3s·+0) | 273 | 257 | 101,064 (무진전 2회 50,559 포함) | **261.2** / 567.4 (사용자 대기 231.7 + 점검표 지정 숨김 계측 74.5 제외) | 8 (Step2 1·Step3 1·4-1 1·4-2 4·4-3 1) + 감사용 3 | 1.71 (스크롤 기준) · 3.43 (무진전 2회 제외) · 0.66 (벽시계 기준) | **true** (탭 생성 직후) |
| 퍽퍽살이 싫어 내가 만든 곱도리 | 86 | 86 | 0 | true | 0 | 1 (77R·19.9s·+80, target 도달) | 77 | 258 | 19,892 | **111.6** / 128.0 (hidden 관찰 대기 16.4 제외) | 5 + 감사용 1 | 4.32 · 0.77 | **true** (Step 2 시점 — 사용자가 Claude 앱에서 답장 중) → 16초 뒤 false |
| 참 제육 | 90 | 90 | 0 | true | 0 | 1 (82R·21.1s·+84, target 도달) | 82 | 258 | 21,140 | **110.0** / 110.0 | 5 | 4.26 · 0.82 | false |

**벽시계 분해 — 시간이 어디서 새는가** (초)

| 매장 | 페이지 로드 + Step 2 (navigate→Step 2 결과) | 사이트 렌더링(`_scroll` elapsed + Step 3 소요) | 도구 왕복·코드 생성(결과 수신→다음 결과 수신 − 페이지 내 소요, Step 3~4-3 구간) | 사용자 대기 |
|---|---|---|---|---|
| 김치찜의 정석 | 23.3 | 104.3 (101.1 + 3.2) | **133.6** (Step 3 블록 전 23.8 · 4-1 블록 전 40.8 · 4-2 4회 7.6/6.5/18.1/11.2 · 4-3 블록 전 25.7) | 231.7(창 꺼내기) |
| 곱도리 | 14.2 | 23.1 (19.9 + 3.2) | **74.3** (20.8 · 32.8 · 6.2 · 14.6) | 16.4(hidden 재확인) |
| 참 제육 | 13.8 | 24.3 (21.1 + 3.2) | **71.9** (20.4 · 33.1 · 6.1 · 12.3) | 0 (계정 A→B 전환 자체는 210.8, 그 안에 P3 계측 포함) |

- 결론: 게시중단이 없는 매장은 **도구 왕복·코드 생성(72~74초)이 사이트 렌더링(23~24초)의 3배**다. 큰 코드 블록(4-1 약 7KB → 33~41초, Step 3 약 2.5KB → 20~24초)을 매장마다 다시 내보내는 구조가 원인이고, 이는 스크롤 최적화(S1·S2·S5)로는 줄지 않는다(→ 속도 개선안 S6). 게시중단이 있는 매장(김치찜)은 종료 확인 50.6초(S1)가 더해져 사이트 쪽이 커진다.
- 2026-09-09 기준값 대비: 190건 약 50초·257ms/라운드·3.7건/초 → 오늘 173건 productive 50.5초·257ms·3.43건/초로 사이트 쪽은 동일 수준. `_scroll` 라운드 분포 251~271ms, 최대 271.
- `_scroll` 1회당 도구 왕복 오버헤드(짧은 호출): 6.1~7.6초(결과만 읽고 재호출) ~ 18초(결과 해석 추가 시).
- 매장별 리뷰번호 집합은 같은 오리진 `localStorage` 키 `_audit_A_kimchi`/`_audit_A_gopdori`/`_audit_A_cham` 에 보관해 실험 결과와 대조했다(navigate·로그아웃/로그인 뒤에도 유지됨 — 실측). 점검 종료 시 삭제.

## 속도 개선안 (최대 5 — 이번 회차 신설)

합격 기준(공통): 같은 매장·같은 세션·hidden false 에서 A(현재 코드) / B(후보) 각 1회, **리뷰번호 집합 동일 + countMatch true + parseFail 동일 + warns 동일 + blocked 동일**, 그리고 필드(stars·date·nickname·menu·reviewText) 전건 동일. 김치찜은 A(기준선) 뒤 새 리뷰 1건(2026092002781715, 2026-09-20, 5점)이 달려 전체(N) 174→175 가 됐다 — 모든 B 결과의 차이는 이 1건(onlyB)뿐이고 onlyA 는 0건이었다. **실험은 전부 페이지 안 `window` 함수 재정의로만 했고 저장소·설치본 SKILL.md 는 건드리지 않았다.**

| # | 후보 | (a) 현재 코드 원문 | (b) 실측 | (c) 예상 절감(초/매장) | (d) 정확성 리스크 · 검증 방법 | 판정 |
|---|---|---|---|---|---|---|
| S1 | target 판정에 `blocked` 포함 + 바닥 도달 조기 반환 | 336: `if (target && a >= target) break;` | [실측] 김치찜 A: 4회·273R·**101.1s**(무진전 2회 50.6s) / B(`_scrollS1`, 250ms 대기 그대로): 2회·164R·**41.7s**, 173+1=175 도달 즉시 종료. 바닥 감지 경로(도달불가 target 으로 호출): 6R·wiggle 2·**3.1s** 에 `bottom:true`. 집합 동일(+신규 1), 필드 diff 0, warns·blocked·parseFail 동일 | 게시중단 ≥1 매장 **≈59** (101.1→41.7); 게시중단 0 매장 0(이미 target 종료). 무진전 확인이 필요한 경우 25s×2 → ≈3s×2 | 로더 지연 중 바닥으로 오판해 조기 반환 → 호출부의 "무진전 2회" 규칙이 재호출하므로 데이터 영향 없음(시간만). scrollHeight 미세 변동(153,506→153,471)으로 첫 wiggle 은 불충족 → `\|Δ\| < 50px` 허용 검토. 검증: 3매장 A/B(오늘 김치찜만 경로 발동) | **개선안** (결함 5의 코드 쪽) |
| S2 | 라운드 대기 250ms 고정 → "변화 감지 시 조기 진행(+50ms 정착), 상한 250ms" 적응형. 폴링은 지문(리뷰번호 span 수·마지막 리뷰번호·scrollHeight)만 보고 `_parse` 는 라운드당 1회 유지 | 327: `await new Promise(r => setTimeout(r, 250));` | [실측] `_parse` 자체 0.2~2ms(중앙값 0.5) → 라운드 257ms 의 99%가 고정 대기. scrollBy 후 scrollHeight 변화 25~65ms(중앙값 42, 53/55R), scrollY 변화 중앙값 42ms; 카드 **수** 변화는 11/55R 뿐(윈도우 12~13 고정 — count 는 신호로 부적합). B(`_scrollB` step 900): 참제육 82R·**8.19s**(A 21.14s) avgRound 100·capHits 0·wiggle 0 / 김치찜 165R·**14.24s**(A productive 50.5s) avgRound 86·capHits 1·wiggle 0. 두 매장 모두 집합·필드·warns·blocked·parseFail 동일 | ≈ 60%: 참제육 13, 곱도리 ≈12(미실측, 비례 추정), 김치찜 36(S1 적용 후 기준) | 정착 50ms 안에 카드 본문이 다 붙지 않으면 필드 누락 가능 → 실측 0건(2매장). 숨김 상태에서는 25ms 폴이 1초로 클램프돼 라운드 ≈1000ms → `throttled` 감지가 유지되어야 하나 **숨김 상태 B 실행은 하지 않았다**(수정 회차 검증 항목). 검증: 3매장 A/B + 숨김 1회 | **개선안** |
| S5 | 스크롤 스텝 900 → 1800px (카드 약 2개) | 326: `window.scrollBy(0, 900);` | [실측] 카드 높이 참제육(90) min 281·p10 728·**중앙값 896**·p90 984·max 1136·평균 859 / 김치찜(174) min 241·p10 656·**중앙값 912**·p90 1008·max 1112·평균 856. DOM 윈도우 8~13카드(중앙값 12 ≈ 10,000px). 900px ≈ 카드 1개/라운드(gained 1 이 48/55R). B2(적응형 대기 + step): 참제육 1800→41R·4.59s / 2700→28R·2.49s / 4500→17R·1.65s; 김치찜 1800→82R·7.03s / 2700→55R·4.95s / 4500→33R·≈3.1s. **전부 집합 동일, wiggle 0, 김치찜 게시중단 1건 포착** | 1800 기준 S2 대비 추가 ≈45%(참제육 8.2→4.6, 김치찜 14.2→7.0). 2700 은 추가 ≈30%p 더 | 스텝이 DOM 윈도우(~10,000px)에 근접하면 카드가 렌더 전에 지나갈 수 있음 → 4500 까지 2매장 누락 0 이나 **한 세션·같은 날 실측**이다. 권장은 보수적으로 1800(윈도우의 1/5). 검증: 3매장 A/B + 다른 날 1회 재실측 | **개선안(조건부: 1800)** |
| S3 | Step 3 적용 후 2.5초 고정 대기 → "이전 값에서 바뀌고 non-null 로 200ms 안정될 때까지 폴링, 상한 3초" | 174: `await new Promise(s => setTimeout(s, 2500));` | [실측] 참제육 적용 클릭 후 50ms 샘플: 915 → **null@76ms**(다이얼로그 open) → 닫힘@136ms → **90@244ms** → 4연속 동일@**469ms**. 3000ms 시점 값(labelOk true·range 8/21~9/20·ET 90)과 동일. 9/9 기록: 252~959ms(김치찜 숨김 상태 959) | ≈ **2.0**(2500→~500) | 조건 불충족 시(30일 건수가 이전 값과 같음, 이미 30일 상태) 상한 3초까지 대기 → 지금보다 0.5초 느릴 뿐. null 과도기(76~187ms)는 non-null 조건이 걸러낸다. 검증: 3매장 ET 동일 | **개선안(순위 낮음)** |
| S6 | 도구 왕복·코드 생성 절감 — (i) Step 2 + Step 3 + 4-1(정의만) 을 한 호출로 병합, (ii) 정의 블록을 같은 오리진 `localStorage` 에 1회 저장 후 다음 매장에서 재주입 | 79~110 / 139~183 / 211~355 세 블록이 매장마다 별도 호출 | [실측] 위 벽시계 분해표: 4-1 블록 전 32.8~40.8s, Step 3 블록 전 20.4~23.8s, 4-3 블록 전 12.3~25.7s — 게시중단 없는 매장에서 도구 쪽 72~74s vs 사이트 23~24s | (i) 왕복 2회분 ≈ **15~30**(추정) (ii) 4-1 재전송 생략 ≈ 30~40 × 2매장(추정) | 수집 로직 무변경이라 결과 집합 영향 없음. (i) 는 hidden 게이트 이전에 Step 3 이 실행되는 순서 변경(Step 3 은 숨김 상태에서도 정상 — 9/9 실측)이므로 **사용자 승인 사항**. (ii) 는 세션 간 stale 코드 위험 → 버전 키 필수. **절감치는 실측 아님[추론]** | **후보(승인 필요)** |

S4(별점 필터): **없음.** 기간 다이얼로그 텍스트 `기간 | 최근 7일 | … | 최근 30일 | … | 최근 3개월 | … | 최근 6개월 | … | 날짜 직접 선택 | 한번에 6개월까지 조회할 수 있어요. | 23년 4월23일 이전 리뷰는 주문유형을 조회할 수 없어요. | 적용`(라디오 5개 `name="review-date-filter"`, 버튼 `닫기`(aria-label)·`적용`), 정렬 메뉴 `리뷰 정렬 | 추천순 | 최신순 | 취소`, 탭 `전체(90)`·`미답변(5)`·`차단(0)`. 별점 관련 문구는 통계 표기 `평균 별점` 만.

하지 않은 것(지시대로): 매장 간 대기·`_dismissAll` 400ms 등 합쳐 2초 미만 항목, 스크린샷 깨우기·browser_batch 사이클, 계정 전환·창 꺼내기 자동화.

### 실험 코드 원문 (수정 회차가 그대로 쓴다)

S1 — `_scrollS1` (원본 `_scroll` 에서 두 곳만 다름: ← 주석 참조):
```javascript
window._atBottom = () => Math.round(scrollY) + innerHeight >= document.body.scrollHeight - 2;
window._scrollS1 = async function(budgetMs = 25000, target = null) {
  const t0 = performance.now(); let rounds = 0, stuck = 0, authExpired = false, throttled = false, bottom = false, wiggles = 0;
  const before = Object.keys(window._all).length; const roundMs = [];
  while (performance.now() - t0 < budgetMs) {
    if (window._lost()) { authExpired = true; break; }
    rounds++; const rt = performance.now(); const b = Object.keys(window._all).length; const shB = document.body.scrollHeight;
    window.scrollBy(0, 900);
    await new Promise(r => setTimeout(r, 250));
    const a = window._parse();
    roundMs.push(Math.round(performance.now() - rt));
    if (roundMs.length >= 3 && roundMs.slice(-3).every(ms => ms > 600)) { throttled = true; break; }
    if (target && a + Object.keys(window._blocked).length >= target) break;            // ← 원본: if (target && a >= target) break;
    if (a === b) {
      if (++stuck >= 3) {
        wiggles++;
        await window._dismissAll();
        window.scrollBy(0, -600); await new Promise(r => setTimeout(r, 200));
        window.scrollBy(0, 1500); await new Promise(r => setTimeout(r, 600));
        const a2 = window._parse(); stuck = 0;
        if (a2 === b && window._atBottom() && document.body.scrollHeight === shB) { bottom = true; break; }   // ← 신설: 바닥 도달 조기 반환
      }
    } else stuck = 0;
  }
  const after = Object.keys(window._all).length;
  return { collected: after, gained: after - before, blocked: Object.keys(window._blocked).length, rounds, wiggles, bottom, throttled, hidden: document.hidden,
    avgRoundMs: roundMs.length ? Math.round(roundMs.reduce((s, x) => s + x, 0) / roundMs.length) : null, authExpired, elapsedMs: Math.round(performance.now() - t0), scrollY: Math.round(scrollY), scrollH: document.body.scrollHeight };
};
```

S2·S5 — 지문 `_fp`, 적응형 대기 `_waitB`, `_scrollB(budget, target, step)` (S1 의 두 변경도 포함):
```javascript
window._fp = () => { let last = '', n = 0; for (const s of document.querySelectorAll('span')) { const t = s.textContent; if (t && t.length < 30 && /^리뷰번호\s+\d+$/.test(t.trim())) { n++; last = t; } } return n + '|' + last + '|' + document.body.scrollHeight; };
window._waitB = async function(capMs = 250, settleMs = 50, pollMs = 25) { const t0 = performance.now(); const f0 = window._fp(); while (performance.now() - t0 < capMs) { await new Promise(r => setTimeout(r, pollMs)); if (window._fp() !== f0) { const rem = Math.min(settleMs, capMs - (performance.now() - t0)); if (rem > 0) await new Promise(r => setTimeout(r, rem)); return Math.round(performance.now() - t0); } } return Math.round(performance.now() - t0); };
window._scrollB = async function(budgetMs = 25000, target = null, step = 900) {
  const t0 = performance.now(); let rounds = 0, stuck = 0, authExpired = false, throttled = false, wiggles = 0, bottom = false;
  const before = Object.keys(window._all).length; const roundMs = [], waitMs = [];
  while (performance.now() - t0 < budgetMs) {
    if (window._lost()) { authExpired = true; break; }
    rounds++; const rt = performance.now(); const b = Object.keys(window._all).length; const shB = document.body.scrollHeight;
    window.scrollBy(0, step);
    waitMs.push(await window._waitB(250, 50, 25));          // ← 원본: await new Promise(r => setTimeout(r, 250));
    const a = window._parse();
    roundMs.push(Math.round(performance.now() - rt));
    if (roundMs.length >= 3 && roundMs.slice(-3).every(ms => ms > 600)) { throttled = true; break; }
    if (target && a + Object.keys(window._blocked).length >= target) break;
    if (a === b) { if (++stuck >= 3) { wiggles++; await window._dismissAll(); window.scrollBy(0, -600); await new Promise(r => setTimeout(r, 200)); window.scrollBy(0, 1500); await new Promise(r => setTimeout(r, 600)); const a2 = window._parse(); stuck = 0; if (a2 === b && window._atBottom() && document.body.scrollHeight === shB) { bottom = true; break; } } } else stuck = 0;
  }
  const after = Object.keys(window._all).length;
  return { collected: after, gained: after - before, blocked: Object.keys(window._blocked).length, rounds, wiggles, bottom, throttled, hidden: document.hidden,
    avgRoundMs: roundMs.length ? Math.round(roundMs.reduce((s, x) => s + x, 0) / roundMs.length) : null, avgWaitMs: waitMs.length ? Math.round(waitMs.reduce((s, x) => s + x, 0) / waitMs.length) : null, waitCapHits: waitMs.filter(w => w >= 250).length,
    authExpired, elapsedMs: Math.round(performance.now() - t0), scrollY: Math.round(scrollY), step };
};
```

A/B 대조 헬퍼(기준선 집합은 4-3 직후 `localStorage.setItem('_audit_A_<매장>', JSON.stringify({ all: window._all, blocked: window._blocked, warn: window._warn, res }))` 로 저장):
```javascript
window._compareA = function(key) {
  const A = JSON.parse(localStorage.getItem(key)); const ak = Object.keys(A.all).sort(), bk = Object.keys(window._all).sort();
  const onlyA = ak.filter(k => !window._all[k]), onlyB = bk.filter(k => !A.all[k]); const fieldDiff = [];
  for (const k of bk) if (A.all[k]) for (const f of ['stars','date','nickname','menu','reviewText']) if (A.all[k][f] !== window._all[k][f]) fieldDiff.push(k + ':' + f);
  return { aCount: ak.length, bCount: bk.length, sameSet: !onlyA.length && !onlyB.length, onlyA: onlyA.slice(0, 5), onlyB: onlyB.slice(0, 5), fieldDiff: fieldDiff.slice(0, 8), warnSame: JSON.stringify(A.warn) === JSON.stringify(window._warn), blockedSame: JSON.stringify(Object.keys(A.blocked).sort()) === JSON.stringify(Object.keys(window._blocked).sort()), parseFailA: A.res.parseFail, parseFailB: Object.values(window._all).filter(x => x.stars < 1 || x.stars > 5).length };
};
```

S3 — 적용 클릭 뒤 샘플링(원본 `_applyPeriod` 의 `await … 2500` 자리를 아래로 바꿔 계측했다; 후보 코드는 `firstNonNull`/`stable200` 조건으로 return 하면 된다):
```javascript
const tApply = performance.now(); apply.click();
const totalOf = () => { const m = document.body.innerText.match(/전체\s*\(([\d,]+)\)/); return m ? parseInt(m[1].replace(/,/g, ''), 10) : null; };
const samples = []; let firstChange = null, firstNonNullAfterChange = null, stableAt = null, closedAt = null, lastVal = before, streak = 0;   // before = 적용 전 totalOf()
while (performance.now() - tApply < 3000) {
  await new Promise(s => setTimeout(s, 50));
  const ms = Math.round(performance.now() - tApply); const v = totalOf(); const op = isOpen();
  if (samples.length < 12 || v !== lastVal) samples.push([ms, v, op ? 'open' : 'closed']);
  if (closedAt === null && !op) closedAt = ms;
  if (firstChange === null && v !== before) firstChange = ms;
  if (firstChange !== null && firstNonNullAfterChange === null && v !== null && v !== before) firstNonNullAfterChange = ms;
  if (v === lastVal && v !== null && v !== before) { streak++; if (streak >= 4 && stableAt === null) stableAt = ms; } else if (v !== lastVal) { streak = 0; }
  lastVal = v;
}
```

S2 계측 프로브 `_probe(budget, target, capMs)` — 라운드마다 25ms 샘플링으로 `[scrollY 변화ms, 카드수 변화ms, scrollHeight 변화ms, 리뷰번호 span 수 변화ms, parseMs, gained, DOM 카드수]` 를 기록(속도 후보가 아니라 측정용):
```javascript
window._probe = async function(budgetMs = 24000, target = null, capMs = 500) {
  const t0 = performance.now(); const log = []; let rounds = 0;
  const sig = () => { const spans = document.querySelectorAll('span'); let n = 0; for (const s of spans) { const t = s.textContent; if (t && /^리뷰번호\s+\d+$/.test(t.trim())) n++; }
    return { cards: document.querySelectorAll('[class*="ReviewContent-module"]').length, sh: document.body.scrollHeight, nos: n }; };
  while (performance.now() - t0 < budgetMs) {
    rounds++;
    const s0 = sig(); const y0 = Math.round(scrollY);
    window.scrollBy(0, 900);
    const rt = performance.now(); let firstCards = null, firstSh = null, firstNos = null, firstScrollY = null;
    while (performance.now() - rt < capMs) {
      await new Promise(r => setTimeout(r, 25));
      const el = Math.round(performance.now() - rt); const s = sig();
      if (firstScrollY === null && Math.round(scrollY) !== y0) firstScrollY = el;
      if (firstCards === null && s.cards !== s0.cards) firstCards = el;
      if (firstSh === null && s.sh !== s0.sh) firstSh = el;
      if (firstNos === null && s.nos !== s0.nos) firstNos = el;
      if (firstCards !== null && firstSh !== null && firstNos !== null) break;
    }
    const pt = performance.now(); const b = Object.keys(window._all).length; const a = window._parse(); const parseMs = Math.round((performance.now() - pt) * 10) / 10;
    log.push([firstScrollY, firstCards, firstSh, firstNos, parseMs, a - b, sig().cards]);
    if (target && a >= target) break;
  }
  return { rounds, collected: Object.keys(window._all).length, blocked: Object.keys(window._blocked).length, elapsedMs: Math.round(performance.now() - t0), log };
};
```

## 정확성 판정 (이번 회차 지시 P1~P7)

- **P1 필터 적용 확인(이월 ③)** → **결함 1로 승격.** 재현 절차·수치는 결함 1 행. 시험한 엄격 판정 코드(기간 **버튼 자신의** 텍스트에서 두 날짜를 읽고, 다이얼로그가 닫혀 있어야 통과):
  ```javascript
  const strict = () => {
    const inView = el => { const r = el.getBoundingClientRect(); return r.width > 0 && r.height > 0; };
    const open = [...document.querySelectorAll('[role="dialog"],[aria-modal="true"]')].some(e => inView(e) && /기간/.test(e.innerText || '') && /최근\s*\d+\s*(일|개월)/.test(e.innerText || ''));
    const btn = [...document.querySelectorAll('button,[role="button"]')].find(e => /최근\s*\d+\s*(일|개월)/.test(e.innerText || '') && inView(e));
    const txt = btn?.innerText || '';
    const mm = txt.match(/(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})[^~]*~\s*(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})/);
    const pad = n => String(n).padStart(2, '0');
    const today = new Date(); const from = new Date(today); from.setDate(from.getDate() - 30);
    const iso = d => `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}`;
    const rs = mm ? `${mm[1]}-${pad(mm[2])}-${pad(mm[3])}` : null, re = mm ? `${mm[4]}-${pad(mm[5])}-${pad(mm[6])}` : null;
    return { dialogOpen: open, btnLabel: txt.split('\n')[0], rangeStart: rs, rangeEnd: re, expectStart: iso(from), expectEnd: iso(today), pass: !open && rs === iso(from) && re === iso(today) };
  };
  ```
  결과: 라디오만 바꾼 열린 상태 `{dialogOpen:true, btnLabel:"최근 6개월", rangeStart:"2026-03-21", expectStart:"2026-08-21", pass:false}`, 닫고 미적용 `pass:false`, 정상 적용(기준선 3매장) 버튼 텍스트 `최근 30일|2026. 8. 21 (금) ~ 2026. 9. 20 (일)` → `2026-08-21 ~ 2026-09-20` 일치. 사이트의 "최근 30일" = 오늘−30일 ~ 오늘(8/21~9/20) 확인. 자정 전후 실행 시 `today` 가 넘어가는 경계는 ±1일 허용을 검토할 것[추론].
- **P2 countMatch 편입** → 3매장 성립(174=173+1, 86, 90), 9/9 포함 6/6. 편입 비용·이득 한 줄씩은 개선안 ① 에. **사용자 결정.**
- **P3 로그아웃 URL 패턴** → **[실측] 판정 완료.** 계정A 로그아웃 후 `https://self.baemin.com/shops/14697934/reviews` navigate → 12.9~16.5초 샘플 8회 모두 `host: biz-member.baemin.com`, `path: /login`, 쿼리 키 `[returnUrl, __ts]`. `/login|signin|auth/i.test(location.href)` = **true**, `noShop` false, `titleOk('참 제육')` false, `document.title` "배민비즈회원", 본문 118자: `통합로그인 |  | 자동 로그인 |  | 아이디 저장 |  | 로그인 | 아이디·비밀번호 찾기 | 아이디로 회원가입 |  | 또는 소셜 계정으로 시작 |  | 카카오 | 네이버 | 애플 | 자동 슬라이드 쇼 중지 | 2 | / | 2 |  | © Woowa Brothers Corp.` → Step 2 의 `isLogin` 과 `_lost()` 의 URL 패턴은 **전면 리다이렉트 경로에는 맞다.** 단 수집 도중(SPA 상태에서) 세션이 끊길 때도 같은 리다이렉트가 일어나는지는 조건을 만들 수 없어 여전히 [추론]. 부수 발견 → 결함 2(반환값 BLOCKED).
- **P4 닉네임 실패 1건** → **결함 4.** DT 에 `픽업` 추가 필요 여부: 이 건과 무관(첫 줄 `알뜰배달`). 픽업 카드는 김치찜 30일에 2건 있고 fallback 으로 정상 → 개선안 ⑤(a) 로 이월(순위 5).
- **P5 더보기·답글** → [실측] 3매장 349건 중 `더보기` 문구 0건. 최장 리뷰 곱도리 2026090102903819 **206자**: DOM 본문 206자 = parsed 206자(문자열 일치), 참제육 최장 2026092002746037 168자도 일치. 카드 안 `-webkit-line-clamp` 는 메타 span(배달유형·닉네임 등, clamp 1)에만 있고 본문에는 없음. 사장님 답글: 카드 줄 순서가 `… 주문메뉴 | <메뉴> | 배달리뷰 | 좋아요 | 사장님 | <답글 날짜> | <답글 본문…> | 삭제 | 수정 | 사장님 댓글 추가하기` 라 `pub`(날짜~주문메뉴 구간) 에 답글이 섞이지 않음 — 답글 2개 카드(2026091002657117)·1개 카드(2026091802618293) 확인. **단 같은 대조에서 결함 3(파트너전용 라벨) 발견.** `(텍스트 없음)` 105/349 건은 샘플 2건 확인 결과 실제 본문 없음(배달리뷰 칩 + 답글만).
- **P6 실행 전 자동 백업** → 개선안 ②(쿠팡 Step 0 블록 이식). **사용자 승인 사항.**
- **P7 양방향 대조표**

| 방향 | 항목(출처) | 상대 스킬 해당 여부 | 근거 |
|---|---|---|---|
| 쿠팡→배민 | 결함 1 `too_old` 날짜 규칙 제거 | **해당 없음** | 배민 5-2(516~524행)는 날짜 규칙 없이 리뷰번호 부재만으로 삭제. N-3 부수효과(모르는 매장 행 영구 잔존)도 배민에 동일하게 존재하나 쿠팡과 같은 이유로 의도된 동작 |
| 쿠팡→배민 | 결함 2·정정 2 잘림 서술 `[TRUNCATED]` 통일 | **해당 — 결함 7** | 배민 419행 `끝이 잘린 JSON`. 이번 세션 3회 관측 |
| 쿠팡→배민 | 결함 3·개선안 1 API 오류 봉투 | 해당 없음 | 배민은 API 미사용 |
| 쿠팡→배민 | 개선안 2 Step 0 자동 백업 | **해당 — 개선안 ②(P6)** | 쿠팡 SKILL.md 47~50행 블록. 배민 Step 5 실패 대체 경로는 교차 정정 회차에 이미 `backup/` 으로 맞춤 |
| 쿠팡→배민 | 개선안 2(구 4) 저장 후 재열기 확인 | **해당 — 개선안 ③** | 두 스킬 모두 없음 |
| 쿠팡→배민 | 개선안 3(구 5) `ratingFail` 비율 임계 | **해당 — 개선안 ④** | 배민 396행 전건 실패만 판정 |
| 쿠팡→배민 | 점검표 개정안 8 audit-only 계측 1줄 허용 | **해당 — 점검표 개정안 3** | 이번 회차가 실제로 필드 추가·localStorage 보관을 썼다 |
| 쿠팡→배민 | 점검표 개정안 9 사본 dry-run 허용 | **해당 — 점검표 개정안 4** | 배민 점검표는 "Step 5 실행하지 마라" 만 |
| 쿠팡→배민 | 정정 1 "바이트 수 적지 않는다, 행수는 `wc -l`" | **해당 — 점검표 개정안 5** | 배민 기준선·개정안 10 에 bytes 표기 있음 |
| 쿠팡→배민 | v2.1 "문구 교체는 파일 전체 검색으로" | **해당 — 점검표 개정안 6** | 배민 [수정 회차에 적용할 것] 에 없음 |
| 배민→쿠팡 | 결함 3 Step 0 세 줄 분리 | **해당 — 쿠팡 미수정** | 쿠팡 SKILL.md 39행 `ls -d $HOME/mnt/claude && python3 -c "import openpyxl;print('openpyxl ok')" && ls $HOME/mnt/claude/'~$쿠팡_저점수리뷰.xlsx' 2>/dev/null && echo LOCKED \|\| echo UNLOCKED` 그대로. 폴더·openpyxl 부재가 `UNLOCKED` 로 찍히고, 47행 "`UNLOCKED`면 실행 전 백업" 이 이어져 백업 블록은 `백업 대상 없음(첫 실행)` 으로 통과한다[추론 — 쿠팡은 이번에 실행하지 않음] |
| 배민→쿠팡 | hidden 게이트·`throttled` | 해당 없음[추론] | 쿠팡은 `fetch` 폴링이라 렌더링 정지 영향 없음. 타이머 클램프로 매장 간 `setTimeout(500)` 이 1초가 되는 정도 |
| 배민→쿠팡 | `countMatch`(collected+blocked=전체) | 해당 없음 | 쿠팡은 `collected === apiTotal` 정확 일치를 이미 씀(쿠팡 개정안 6) |
| 배민→쿠팡 | 결함 2 반환값에 `location.href`(쿼리) → `[BLOCKED]` | **해당 검토** | 쿠팡 Step 1 주석 68행이 같은 현상을 이미 알고 URL 을 결과에 안 담는다 — 쿠팡 쪽은 이미 대응됨 |

## 실행 실측 기록

- 실행 시각(UTC): 김치찜 navigate 11:35:07 → 4-3 결과 11:44:34 / 곱도리 11:45:55 → 11:48:03 / 참제육 11:51:33 → 11:53:23(KST +9h). 그 뒤 참제육 실험 ~12:05, 계정A 재로그인 후 김치찜 실험 12:08~12:13, 곱도리 P4 12:13~12:14.
- 표준 계측: 탭 생성 직후 `hidden: true`. 숨김: `setTimeout(250)` 1257/995/1007/996/995ms, rAF 2000ms 내 미도착, `_scroll(25000,null)` 3R [1129,1005,990] → throttled true 3125ms gained 0(6개월 목록 7카드 고정). 가시: 254/260/255/250/250ms, rAF 16ms. 곱도리 Step 2 시점 hidden true(사용자가 Claude 앱에서 답장 중) → 16초 뒤 false — **사용자가 Claude 앱을 보는 동안은 크롬이 가려져 hidden 이 재발한다**(스킬이 채팅 요청을 보낼 때마다 생길 수 있는 상태).
- Step 2: 3매장 `dismiss: none`, `shopIdOk·titleOk true`, 팝업 0건. Step 3: 3매장 `waited 300`, `labelOk true`, range 동일, 소요 3.15~3.20초. 6개월 기본값 → 30일.
- 별점 소스: 349/349 SVG 색상 경로(aria-label 0). parseFail 0. 카드 경계 `ReviewContent-module` 349/349, `no_card`·`merged` 0. warns 합계 1(`nick_fail:2026082400693614`).
- 배달유형 첫 줄 분포(30일): 김치찜 알뜰배달 149/한집배달 21/가게배달 2/**픽업 2**; 곱도리 알뜰배달 74/한집배달 10/가게배달 2; 참제육 미집계. `PK` 신규 키워드 없음. 파트너전용 라벨 `파트너님에게만 보이는 리뷰입니다.` 9건(결함 3).
- 게시중단 카드(김치찜 2026082401079865) 구조: `알뜰배달 | 빨리빨ㄹ | 2026년 8월 24일 | 리뷰번호 … | 게시중단 요청으로 인해 30일간 임시차단 되었어요 | 원문보기 | 사장님 | 2026년 8월 26일 | …`. "30일간" 이라 9/23 경 차단이 풀리면 정상 수집 대상이 된다(의도된 동작).
- 저점수: **참 제육 1건(신규)** — 2026092001471500 / 2점 / 2026-09-20 / Kimsunglim / `1인 간장 제육 한상` / `돼지냄새가 너무 심해요……`. 김치찜·곱도리 0건. **Step 5 미실행이라 엑셀에는 반영되지 않았다**(사용자에게 채팅으로 보고).
- 뷰포트: innerWidth 1920, innerHeight 911(세션 초) → 855(이후). `javascript_tool` 반환 약 1,000자 초과 시 끝에 `[TRUNCATED]`(3회). `location.href` 에 쿼리스트링이 있으면 결과 전체 `[BLOCKED: Cookie/query string data]`(2회).
- 고정 요소(참제육 @855): 하단 배너 z101 (598,715)~(1308,839) · LNB 하단 (0,711)~(240,855) z2 · FloatingControls (1785,743)~(1865,823) z101 · ChatRoom (1405,111)~(1821,659) z103 · 클래스 없는 전면 요소 (0,0)~(1905,855) z2147483646(확장 오버레이로 추정, `.click()` 경로 영향 없음).
- CDP 45초 타임아웃: 0회(최대 호출 25.5초).
- F 실험(5분 숨김 intensive throttling): 연쇄 `setTimeout(250)` 로거를 12:14 UTC 부터 가동. **첫 582초 동안 hidden 은 한 번도 true 가 되지 않아**(가시 상태 gap 평균 255ms·최대 290ms) 측정 불가 → 사용자에게 창을 5분 이상 가려 달라고 요청함. 결과는 아래 "F 실험 결과" 에.

**F 실험 결과 (2026-09-20 12:27~12:36 UTC, 김치찜 탭, 사용자가 다른 탭으로 가려 `hidden: true`)**

- 로거(고전적 연쇄: 콜백 안에서 `setTimeout(tick, 250)` 재예약, 가시 상태로 13분 가동 후 숨김): 숨김 직후 **0~60초는 1초 클램프**(30초 버킷 평균 979/1000ms, 최대 1018) → **약 70초 시점 11,004ms 간격 1회 → 그 뒤 60,000ms 간격으로 정렬**(7회 연속 59,991~60,005ms, 숨김 430초까지 지속). 즉 이 환경에서 intensive wake-up throttling(분당 1회)은 **숨김 5분이 아니라 약 1분 뒤**에 시작했다[실측].
- 같은 숨김 상태에서 **새 호출로 시작한** `await new Promise(r => setTimeout(r, 250))` 는 1231ms(1초 클램프)였고, 이를 **6회 연쇄**(스킬 `_scroll` 과 같은 패턴, 중첩 1→6)해도 342/1004/998/1002/1000/991ms 로 **60초 정렬이 걸리지 않았다**[실측, 총 5.3초]. 도구 호출은 45초를 넘길 수 없어 "await 연쇄가 숨김 60초 이상 지속되면 정렬되는지"는 측정할 수 없었다.
- 해석: `_scroll` 은 호출마다 새 task 로 시작하고 배치가 25초 이하이며 `throttled`(3라운드 연속 >600ms)가 약 3초에 반환하므로, **현재 코드에서는 60초 정렬에 닿기 전에 반환한다**. 직전 세션의 45초 CDP 타임아웃 기전으로 intensive throttling 은 오늘 데이터로는 **약해졌다** — 남는 후보는 숨김 탭의 렌더러 동결(Chrome 탭 freezing/에너지 절약)로 evaluate 자체가 응답하지 않는 경우[추론]. 계속 미확정.
- 스킬에 대한 함의: 4-1 의 hidden 게이트·`throttled` 조기 반환을 유지해야 하는 이유가 하나 더 생겼다(숨김 1분이 지나면 어떤 연쇄 타이머든 분당 1회로 떨어질 수 있다). S2·S5 의 적응형 대기(25ms 폴)도 같은 `roundMs.length >= 3` 검사를 유지해 중첩이 커지기 전에 반환한다 — 숨김 상태 실행은 수정 회차 검증 항목.

## 미확정으로 남긴 것

- 수집 도중(SPA 유지 상태) 세션 만료 시에도 전면 리다이렉트가 일어나 `_lost()` 가 잡는지 — 조건을 만들 수 없었다. P3 는 페이지 진입 시점의 리다이렉트만 실증.
- Step 3 적용 클릭이 실제로 조용히 무효가 되는 빈도 — 4회 × 3매장 0건. 결함 1 은 "판정 로직이 그 실패를 못 잡는다"는 사실만 재현한 것이다.
- 45초 CDP 타임아웃 기전(직전 세션) — 이번에도 미재현. F 실험은 위 참조.
- S5 큰 스텝(2700·4500)의 안전 여지가 다른 날·다른 부하에서도 유지되는지 — 같은 세션 실측만 있다.
- `expectedTotal === null` 강제 시 흐름 — 미실측(이월).
- 파트너전용 카드 중 `주문메뉴` 가 없는 2건이 왜 없는지(비공개 리뷰의 표시 규칙인지) — 표본 2건뿐.

## 점검표 개정안

1. **"속도 기준선"·"속도 개선안" 절 상시화 여부.** 이번 회차가 처음 넣었다. 상시화하면 매 회차 (i) 매장별 표(위 열 구성) (ii) 벽시계 분해(페이지 로드 / 사이트 렌더링 / 도구 왕복·코드 생성 / 사용자 대기) (iii) 후보별 (a)(b)(c)(d) + A/B 합격 기준을 고정 형식으로 적게 된다. **비용:** 회차당 A/B 실험 10~20분과 계정 재로그인 1회. **권고:** 기준선 표와 벽시계 분해만 상시(계측은 원래 실행에 필드 몇 개 얹는 수준), 실험 절은 "사용자가 속도를 목표로 지시한 회차에만".
2. **리뷰번호 집합 보관 방법 고정:** 4-3 직후 같은 오리진 `localStorage` 에 저장 → navigate·재로그인 뒤에도 남아 A/B 대조가 페이지 안에서 끝난다(이번 회차 실측). 점검 종료 시 삭제. 도구 반환 1,000자 한계를 우회하는 유일한 저비용 방법.
3. **audit-only 계측 허용 규칙 명시**(쿠팡 개정안 8 이관): 반환 JSON 에 타이밍 필드 추가·localStorage 대입 1~2줄은 허용, 판정 로직(`ok`·`reason`·종료 조건·`_parse`)은 불가침. 넣은 줄은 기준선에 적는다.
4. **사본 dry-run 허용**(쿠팡 개정안 9 이관): "Step 5 는 실제 파일 대상으로는 실행하지 마라, 엑셀 사본에 dry-run 은 한다" — 이번엔 저점수 1건이 있었으므로 dry-run 이 실제 저장 로직을 검증할 기회였는데 하지 않았다.
5. **바이트 수를 기록에 적지 않는다**(쿠팡 정정 1 이관). 무결성은 `wc -l` 행수 + md5 로. 단 이 파일은 마지막 줄 개행이 없어 `wc -l` 637 / 실제 638 이므로 **행 번호를 인용할 때 어느 기준인지 적는다**(이번 기준선은 실제 줄 번호).
6. **문구 교체는 파일 전체 검색으로**(쿠팡 v2.1 이관) — [수정 회차에 적용할 것] 에 추가. 결함 5·6·7 처럼 같은 취지 문장이 여러 곳(26·334·365·370 등)에 있다.
7. **C 항목에 "도구가 결과를 차단·절단하는 경로" 추가:** 반환값에 쿼리스트링 포함 URL 이 들어가면 `[BLOCKED]`, 1,000자 초과면 `[TRUNCATED]`. 결함 2 가 이 유형.
8. **③ 실행 준비 조건에 추가:** "점검 중 사용자에게 메시지를 보낼 때마다 사용자가 Claude 앱을 보면 크롬이 가려져 hidden 이 재발할 수 있다 — 매장 Step 2 마다 hidden 을 다시 본다"(곱도리 실측).
9. **F 실험 절차 구체화:** 정상 계측이 끝난 뒤 연쇄 `setTimeout(250)` 로거를 심고 사용자에게 5분 이상 가려 달라고 요청, 분 단위 gap 버킷으로 읽는다(이번 회차 코드 재사용). 사용자가 Claude 앱을 안 보는 환경(두 모니터 등)에서는 자연 발생하지 않는다.
10. **B 항목 "하단 배너 겹침" 격하:** 2회 연속 겹침 없음·JS `.click()` 경로 무관 → "다이얼로그 개폐 상태별 `elementFromPoint`" 를 매 회차 필수에서 "레이아웃 변경 의심 시에만" 으로.
11. **B 항목에 "파트너전용 라벨 카드 1건 대조" 추가:** 결함 3 유형은 저점수가 희소해 저장 결과로는 드러나지 않는다. 3매장 파트너전용 리뷰번호 목록(위 결함 3)을 표본으로.
12. **[의도된 동작]에 추가:** "`(텍스트 없음)` 은 본문이 없는 리뷰가 맞다(30% 안팎). 배달리뷰 칩(`좋아요/아쉬워요`)은 본문이 아니라 기록하지 않는다" — 다음 회차가 결함으로 올리지 않도록. (개선안 후보로만 남김)
13. **머리말 버전 줄:** v4 → v5 로 갱신하며 이번 회차 채택분을 적을 것(2차 커밋).

## 수정 기록 (2026-09-20 수정 회차 — 사용자가 고른 항목만)

사용자 지시로 진단 세션이 이어서 수정함(검증은 별도 세션). 대상: **결함 1~7 전부**, 개선안 **①②③④⑤(a)** + 추가 1건(`[배달리뷰]` 칩), 속도 **S1·S2·S3·S5(1800px)·S6**, 점검표 개정안 **1~13 전부**(v5). 이월: 개선안 ⑤(b) `titleOk` 헤더 한정.

- 수정 대상: 저장소 `SKILL.md` **637행 → 749행(`wc -l`; 새 파일은 마지막 줄에 개행이 있어 실제 줄 수도 749)**, md5 `5af791ce…` → 이 커밋의 값. `audit/checklist.md` v4 → v5. 설치본은 손대지 않았다.
- **재패키징·재업로드는 하지 않았다**(사용자 지시). 검증은 다른 세션에서 받는다. 재업로드 전에는 설치본(md5 `5af791ce…`, 637행)이 그대로 실행된다.
- 절 번호가 바뀌었다: 옛 `Step 3`(기간 필터)·`4-1`(정의)은 **Step 2**로 병합, 옛 `4-2`(스크롤)는 **Step 3-2**(앞에 **3-1 hidden 게이트**), 옛 `4-3`(저점수 추출)은 **Step 4**. 아래 행 번호는 **749행 파일 기준**이며 절·함수명을 함께 적는다.

### 항목별 변경

| 항목 | 저장소 SKILL.md 변경(절·함수) | 실측 |
|---|---|---|
| 결함 1 (필터 적용 오탐) | **Step 2 `_applyPeriod`**(123~181행): 반환에 `filterOk = !open && startOk && endOk` — 다이얼로그 닫힘 + **기간 버튼 자신의** 텍스트에서 읽은 두 날짜가 오늘−30일 ~ 오늘, 날짜 비교는 `Math.abs(...) <= 1`(±1일, 자정 경계). `labelOk`는 참고 필드로만 남김. 판정 문구(411~414행)와 원칙 3(41행 — 검증 회차 정정: 38행은 빈 줄)을 같은 규칙으로. 함수 시그니처가 `(label, days, skipOpen, maxWait)`로 바뀌어 fallback 호출도 `_applyPeriod('최근 30일', 30, true)`로 갱신 | **정상 경로 3매장 `filterOk: true`**(range `2026. 8. 21 ~ 2026. 9. 20`, totalWaitMs 439/498/433). **실패 재현:** 김치찜 적용 상태에서 다이얼로그 열고 6개월 라디오만 클릭 → 신 `filterOk: false`(dialogOpen true), 구 `labelOk: true`; 곱도리 fresh reload(6개월) + 30일 라디오만 클릭·2.8초 → 신 `false`(range 3/21~9/20), 구 `labelOk: true`; 닫기 후 `false`(미적용이 맞음) |
| 결함 2 (반환값 BLOCKED) | **Step 2 `_prepare`**(358~369행): 반환에 `path: location.host + location.pathname`만, `url` 제거. `shopIdOk`는 `location.pathname.includes('/shops/<id>/')`. 판정(399행)에 "`[BLOCKED:`로 시작하면 로그인 필요로 간주" 추가. 트러블슈팅 행 추가 | 로그아웃 리다이렉트 상태에서 신 Step 2 실행 → 차단 없이 `{isLogin:true, noShop:false, titleOk:false, shopIdOk:false, path:"biz-member.baemin.com/login", stage:"login"}` 반환(2회) |
| 결함 3 (파트너전용 라벨) | **Step 2 `_parse`**(190행 `PK_LABEL`, 255~279행): 라벨 줄은 `partnerOnly` 플래그. `pub`이 비고 라벨이 있으면 라벨 다음 줄~`주문메뉴|배달리뷰|사장님` 전까지를 본문으로. 옛 `partner` 경로는 `!partnerOnly`일 때만(별도 비공개 구간 구조 대비). `review = (partnerOnly ? '[파트너전용] ' : '') + pub` | 3매장 파트너전용 9건 전부 `[파트너전용] 본문` 1회(A/B 대조에서 `partner` 분류 5+3+1). 주문메뉴 없는 2건(2026091102615435 등)도 본문 전문 |
| 결함 4 (닉네임 금지어) | **Step 2 `_parse`**(192행 `META`): 부분 일치 `/주문\|리뷰번호\|배달리뷰\|답글\|사장님/` → 정확 패턴 목록 `[/^\d+회\s*주문\s*고객$/, /^리뷰번호/, /^배달리뷰$/, /^사장님/, /^답글/, /^주문메뉴$/, /^\(최근/, /^\d{4}년\s*\d{1,2}월/, PK_LABEL]`. **원안(5패턴)보다 패턴 4개를 더 넣었다**(`/^답글/`·`/^주문메뉴$/`·날짜 줄 `/^\d{4}년\s*\d{1,2}월/`·라벨 줄 `PK_LABEL` — 검증 회차 정정: `(최근`은 원안에 이미 있었고 `답글`이 빠져 있었음; `/^사장님$/`→`/^사장님/` 앵커 완화도 포함) — 닉네임 줄이 비었을 때 다음 줄(날짜)을 닉네임으로 잡는 경로를 막기 위해 | 곱도리 2026082400693614 닉네임 `내가주문한` 복구, `nick_fail` 0. 3매장 349건 중 닉네임 A/B diff는 이 1건뿐 |
| 결함 5 (종료 조건 문서-코드) | **Step 2 `_scroll`**(333행): `if (target && a + Object.keys(window._blocked).length >= target) break;`. **Step 3-2**(442·449·453행) 문구를 한 문장으로 통일: "target(= collected + blocked ≥ expectedTotal) 도달 또는 `bottom: true` 연속 2회". 상한 8회 → **4회**(25초 배치 ≈ 270라운드·480,000px ≈ 카드 500건) | 김치찜 175+1=176에서 즉시 break(83R·6.9초). 바닥 경로는 검증 회차 실측 1.6초(3R·wiggle 1; 진단 회차의 3.1초는 `<50px` 허용 전 프로토타입 6R·wiggle 2 값 — 검증 회차 정정. SKILL.md 341·449행의 "실측 3.1초" 문구는 그대로 둠) |
| 결함 6 (5.5초 문구) | **Step 2 `_scroll` 주석**(330행): `실측: 3.1~5.5초에 반환 — 첫 라운드 콜드 스타트 유무로 변한다. 판정 기준은 시간이 아니라 3라운드 연속 600ms 초과` | 파일 전체 `grep "5.5초"` → 이 줄만 |
| 결함 7 (TRUNCATED) | **Step 4**(503행) `만약 반환값 끝에 \`[TRUNCATED]\` 표식이 붙어 잘렸으면(약 1,000자 초과) 그때만 폴백한다`, 설계 근거 불릿(30행), 트러블슈팅 행 추가 | `grep "끝이 잘린"` → 0건 |
| 개선안 ① (완전성 판정) | **Step 4**(474~481행): `sum = collected + blocked`; `sum < ET → ok:false '누락 의심 — …'`; `ok && sum > ET → warn '… 수집 중 신규 등록 추정'`(ok 유지). 98% 규칙 삭제. 반환에 `warn` 추가, 최종 보고 표 상태 열에 `warn` 그대로. 원칙 3(41행 — 검증 회차 정정: 38행은 빈 줄) 문구 갱신 | 정상 3매장 `ok:true countMatch:true`. **실패 재현(김치찜 수집 완료 상태, 판정 함수만 다른 ET로):** ET 177 → `ok:false "누락 의심 — 수집 175 + 게시중단 1 < 전체 177"`, ET 175 → `ok:true warn "수집 175 + 게시중단 1 > 전체 175 — …"`, ET null → `ok:false`, ET 0·0건 → `ok:true` |
| 개선안 ② (Step 0 백업) | **Step 0**(65~71행): 쿠팡 블록 이식 `mkdir -p … && if [ -f … ]; then cp … backup/배민_저점수리뷰_$(date +%Y%m%d_%H%M%S).xlsx …; else echo "백업 대상 없음(첫 실행)"; fi`, `백업 실패`면 Step 5 금지. Step 5 문구(538행) "되돌리기는 Step 0이 남긴 backup/ 사본으로" | PC 실행: `백업 완료: 배민_저점수리뷰_20260920_142813.xlsx`, 없는 경로 분기 `백업 대상 없음(첫 실행)` |
| 개선안 ③ (저장 후 재열기) | **Step 5-2**(658~666행): 저장 뒤 `openpyxl.load_workbook(saved)`로 다시 열어 행수·(매장명, 리뷰번호) 집합이 `kept + new`와 같은지 → 반환 `verified`·`file_rows`. 판정·최종 보고에 `verified: false` 처리 추가 | 사본 dry-run T1~T4 전부 `verified: true`(아래) |
| 개선안 ④ (parseFail 비율) | **Step 4**(480행): `else if (parseFail > all.length * 0.1) { ok = false; reason = '별점 파싱 실패 N/M — 10% 초과' }`(전건 실패 판정 뒤) | 김치찜 175건에 stars=-1 을 20건(11.4%) 주입 → `ok:false`, 10건(5.7%) → `ok:true parseFail 10`, 전건 → `전건 파싱 실패` |
| 개선안 ⑤(a) (DT 픽업) | **Step 2 `_parse`**(186행) `DT`에 `'픽업'` 추가 | 김치찜 픽업 2건 닉네임 1차 전략으로 파싱(diff 없음 — 이전엔 3차 fallback) |
| 추가 (배달리뷰 칩) | **Step 2 `_parse`**(280~287행): `chip` = `배달리뷰` 다음 줄부터 `사장님|삭제|수정|날짜` 전까지; 본문·partner가 모두 없을 때 `review = '[배달리뷰] ' + chip`. **옛 `[태그만 있는 리뷰]` 라벨(주문메뉴 없는 카드 전용)을 `[배달리뷰]`로 통일했다** — 사용자 지시는 칩 라벨만이었으나 같은 정보에 라벨이 두 개면 엑셀에서 갈리므로 하나로 | A/B 대조: 칩 56+17+33 = 106건, 라벨 개명 2건(2026083000617551 등). 예 `[배달리뷰] 아쉬워요, 매우 늦게 도착` |
| S1 (blocked·바닥) | 위 결함 5 + **Step 2 `_scroll`**(342행) `if (a2 === b && window._atBottom() && Math.abs(document.body.scrollHeight - shB) < 50) { bottom = true; break; }` — `\|Δ\| < 50px`(사용자 지시) | 김치찜 target 즉시 종료(무진전 호출 0회) |
| S2·S5 (적응형 대기·1800px) | **Step 2 `_fp`·`_wait`·`_scroll`**(302~356행): 기준선의 `_scrollB`를 `_scroll`로 채택(`_waitB`→`_wait`). `step` 기본값 1800. 반환에 `wiggles`·`bottom`·`avgWaitMs`·`waitCapHits` | A/B: 김치찜 83R **6.9s**(A 50.3s 2회), 곱도리 40R **3.9s**(A 20.0s), 참제육 43R **4.2s**(A 21.1s). **숨김 1회(참 제육, 사용자가 다른 탭으로 가림):** 3R 975/994/1008ms → `throttled: true` 2.98초, waitCapHits 3 — 감지 유지 |
| S3 (전체(N) 폴링) | **Step 2 `_applyPeriod`**(163~170행): 고정 2.5초 → 50ms 폴링, 이전 값과 다른 non-null이 4연속(200ms)이면 진행, 상한 3초 | totalWaitMs 439 / 498 / 433 (매장당 약 2.0초 절감), ET 176·87·91 정확 |
| S6 (호출 병합·재주입) | **Step 2**를 2-A 전체판(정의 + `_prepare` + `_saveDefs`, 92~384행)과 2-B 재주입판(386~393행)으로. `_prepare`가 팝업·로그인·필터·리셋·첫 파싱을 한 호출로, `_saveDefs(sid)`가 함수 10개의 `toString()`을 `localStorage['_bm_defs']`에 `{sid, src}`로 저장, 2-B는 `sid`가 같을 때만 `eval`. **`SID`는 Step 1에서 세션마다 정한다**(75~84행). hidden 게이트는 **Step 3-1**로 이동. **덧붙인 것:** `stage === 'ready'`일 때만 저장(382행) — 로그인 리다이렉트 중 돌리면 `biz-member` 오리진에 저장돼 쓸 수 없다(실측). `dialogOpen: true`면 재시도는 `skipOpen=true`로(414행) | `eval` 허용 확인(self.baemin.com·biz-member 둘 다). 곱도리·참제육 2-B 재주입 성공(srcLen 16,509 — 검증 회차 재측정, 함수 10개 `toString()`이 주석까지 담음; 수정 회차 원문 13,767은 오기), 1차 김치찜은 로그인 리다이렉트 때문에 `NO_DEFS` → 2-A 재실행(문서대로). **벽시계(신 흐름, 사용자 대기 0):** 곱도리 navigate→Step 4 **54.7s**(구 128.0s), 참 제육 **35.7s**(구 110.0s). 김치찜은 A 실행이 사이에 끼어 총 벽시계 비교 불가(Step 2 페이지 내 1.4s, `_scroll` 6.9s) |
| 점검표 v5 | 개정안 1~13 전부 + "되돌리면 안 되는 것" 22행(신설 8행: filterOk·path·완전성 규칙·PK_LABEL·META·칩·적응형/1800/바닥·병합/재주입·백업/검증), A·B·C·F 항목·[의도된 동작]·[판정 기준]·[수정 회차에 적용할 것] 갱신, 기준선 형식에 "속도 기준선(상시)"·"속도 개선안(속도 목표 회차만)" 절 추가 | — |

### 전/후 속도표 (같은 열 — 후는 신 코드, 같은 날 같은 세션)

| 매장 | 시점 | expectedTotal | collected | blocked | countMatch | parseFail | `_scroll` 호출 | 라운드 합 | avgRoundMs | elapsedMs 합 | 벽시계 navigate→Step 4(사용자 대기 제외) | 도구 호출(스킬 규정분) | 건/초(스크롤·벽시계) | hidden 시작 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | 전(11:35) | 174 | 173 | 1 | true | 0 | 4 | 273 | 257 | 101,064 | 261.2s | 8 | 1.71 · 0.66 | true |
| 〃 | 후(21:53) | 176 | 175 | 1 | true | 0 | **1** | **83** | **83** | **6,932** | 혼재(비교용 A 실행 삽입·로그인 대기) — Step 2 페이지 내 1.4s, scroll 6.9s | 3(+NO_DEFS 1) | 25.2 · — | false(로그인 후) |
| 곱도리 | 전(11:45) | 86 | 86 | 0 | true | 0 | 1 | 77 | 258 | 19,892 | 111.6s | 5 | 4.32 · 0.77 | true→false |
| 〃 | 후(21:58) | 87 | 87 | 0 | true | 0 | 1 | **40** | **96** | **3,852** | **54.7s** | **3** | 22.6 · 1.59 | false |
| 참 제육 | 전(11:51) | 90 | 90 | 0 | true | 0 | 1 | 82 | 258 | 21,140 | 110.0s | 5 | 4.26 · 0.82 | false |
| 〃 | 후(22:02) | 91 | 91 | 0 | true | 0 | 1 | **43** | **98** | **4,214** | **35.7s** | **3** | 21.6 · 2.55 | false |

벽시계 분해(초, 후):

| 매장 | 페이지 로드 + Step 2(navigate→Step 2 결과) | 사이트 렌더링(`_scroll` + 필터 대기 ≈1.1) | 도구 왕복·코드 생성 | 사용자 대기 |
|---|---|---|---|---|
| 곱도리 | 14.2 (전 14.2) | 5.0 (전 23.1) | **35.5** (전 74.3) — 2-B→scroll 12.4 · scroll→Step 4 24.2 | 0 |
| 참 제육 | 9.3 (전 13.8) | 5.3 (전 24.3) | **21.1** (전 71.9) — 6.3 · 15.9 | 0 |

- 도구 왕복은 매장당 72~74초 → 21~36초. 남은 오버헤드의 대부분은 **Step 4 블록(약 2KB) 생성**(16~24초)이다 — 이것도 정의에 함수로 넣고 한 줄 호출로 바꾸면 더 줄어든다(다음 회차 후보, 이번엔 미채택).
- 사이트 렌더링은 매장당 23~24초 → 5초. 게시중단 매장(김치찜)은 101초 → 6.9초.

### A/B 결과 집합 대조 (같은 세션, 구 코드 A = 옛 `_parse`+옛 `_scroll` 900/250 → `localStorage._audit_A_*`, 신 코드 B → `_audit_B_*`)

| 매장 | 집합 | stars·date·menu | nickname | reviewText 차이(전부 의도한 변경) | blocked | warns A → B |
|---|---|---|---|---|---|---|
| 김치찜 | 175 = 175 (onlyA 0, onlyB 0) | 0 | 0 | 62 = 칩 56 + 파트너전용 5 + 라벨 개명 1 | 동일(1) | [] → [] |
| 곱도리 | 87 = 87 | 0 | 1 (`''` → `내가주문한`, 결함 4) | 20 = 칩 17 + 파트너전용 3 | 동일(0) | [nick_fail] → [] |
| 참 제육 | 91 = 91 | 0 | 0 | 35 = 칩 33 + 파트너전용 1 + 라벨 개명 1 | 동일(0) | [] → [] |

진단 회차의 `_audit_A_*`는 진단 종료 시 지웠으므로 이번 회차에 같은 세션에서 다시 만들었다(구 코드 그대로). 검증 회차 대조용으로 `_audit_A_*`·`_audit_B_*`·`_bm_defs`(sid `20260920T2140`)를 이번엔 남겨 두었다.

### 엑셀 사본 dry-run (PC `$HOME/skillwork/dryrun.xlsx`, 신 5-2 스크립트, 실제 파일 md5 `6dae739a…`·mtime 01:41:54 전후 불변)

| 시험 | 입력 | 결과 |
|---|---|---|
| T1 | 3매장 `ok:true`, 참 제육 저점수 1건(2026092001471500 2점) | `new 1, deleted 0, rows 1, file_rows 1, verified true` — 신규 행 `참 제육 / 2026-09-20 / ★★☆☆☆ / Kimsunglim / 1인 간장 제육 한상 / 돼지냄새가 너무 심해요……` |
| T2 | 같은 JSON 재실행 | `new 0, deleted 0, rows 1, verified true`(멱등) |
| T3 | 참 제육 `ok:false` | `untouched 1, rows 1, skipped [[참 제육, 테스트 — 누락 의심]], verified true`(행 보존) |
| T4 | 참 제육 `reviews:[]`, `ok:true` | `deleted 1, rows 0, verified true`(동기화 삭제 동작) |

### 수정 회차 검사

- 저장소 `SKILL.md` 코드 블록 13개: javascript 5개 `node --check`(플레이스홀더 치환·async 래핑) 통과, bash 4개 `bash -n` 통과, 5-2 python heredoc `py_compile` 통과, 코드펜스 26개(짝수), UTF-8 정상, `name: baemin-review` 유지.
- 전체 검색(파일 전체 grep, 수정 전/후): `끝이 잘린` 0, `5.5초에 반환` → 정정 문구 1곳만, `98%` → 역사 주석 1곳만, `url: location.href` 0, `[태그만 있는` 0, `Step 4-1/4-2/4-3` 0, 옛 `gained === 0 연속 2회` 0, `labelOk` 로 판정하는 문장 0(참고 필드 설명만).
- 파일을 다시 열어 Step 0·2 판정·3·4 절을 눈으로 확인했다.

### 원안을 바꾼 곳과 이유

1. 개선안 ①은 진단 회차의 "정확 일치" 제안 대신 **사용자 지시대로** `<` 실패 / `>` 경고로 넣었다(수집 중 신규 등록을 실패로 만들지 않기 위해).
2. `[태그만 있는 리뷰]` 라벨을 `[배달리뷰]`로 **통일**했다(위 표 "추가" 행). 지시 범위 밖이지만 라벨 두 개가 같은 정보를 가리키는 것을 피하려는 것이고 실측 영향은 2건.
3. `META`에 원안(5패턴)보다 4패턴(`답글`·`주문메뉴`·날짜 줄·`PK_LABEL`)을 더 넣었다(위 결함 4 행. 검증 회차 정정 — 원문 "원안(4패턴)"·"날짜·(최근·주문메뉴·라벨"은 열거 오기).
4. `_saveDefs`를 `stage === 'ready'`일 때만 호출하고, `dialogOpen: true`면 재시도를 `skipOpen=true`로 — 둘 다 실행 중 관찰로 추가.
5. 4-2 상한 8회 → 4회(사용자 지시 "새 동작에 맞게 정합"에 따라 계산 근거를 적음).

### 검증 회차가 볼 것

- 3매장 정상 경로: Step 2 `filterOk: true`, `_scroll` 1회 종료, Step 4 `ok:true countMatch:true warn ''`, 파트너전용 카드 `[파트너전용] 본문` 1회, 칩 `[배달리뷰] …`, 곱도리 2026082400693614 닉네임 `내가주문한`.
- 실패 재현: 라디오만 바꾸고 미적용 → `filterOk:false`; ET+1 → `누락 의심`; ET−1 → `warn`; parseFail 11% → `ok:false`.
- 숨김 상태 `_scroll` → 3라운드 `throttled:true`. 2-B 재주입 `NO_DEFS` 경로(다른 SID).
- 사본 dry-run T1~T4 재현, 실제 파일 md5 불변.
- 설치본은 아직 옛 버전(637행)이어야 정상 — 재업로드 전.

## 수정 기록 2 (2026-09-23 수정 회차 2 — 검증 회차(2026-09-21) 결과 처리)

사용자 지시: 검증 회차가 재현 실패로 올린 1건(숨김 상태 가짜 `bottom:true`)만 코드로 고치고, 기록 문구 오차 4건은 기록만 정정, 그 외는 손대지 않음. 재패키징·재업로드는 하지 않았다(검증을 다시 받는다). 설치본은 여전히 637행(`5af791ce…`).

- 수정 대상: 저장소 `SKILL.md` **749행 → 762행(`wc -l`; 마지막 줄 개행 있음)**, md5 `41b3822f…` → 이 커밋의 값. `audit/checklist.md` v5 → v6("되돌리면 안 되는 것" throttled 행·[실행 준비 조건] 숨김 계측). 이 파일의 2026-09-20 수정 기록 4곳 정정(아래).
- **행 번호는 762행 파일 기준**이며 절·함수명을 함께 적는다.

### 검증 회차(2026-09-21, 사용자 폴더 `baemin-review_검증_2026-09-21.md`) 결과 요약

- 대상 커밋 094d939. 결함 1~7·개선안 ①②③④⑤(a)+칩·S1·S3·S6·점검표 v5·수정 회차 검사 전부 재현. 3매장 A/B(구 코드 설치본 637행 vs 신 코드 749행, 같은 세션) 집합 170=170·85=85·89=89, 필드 차이는 의도한 것뿐(칩 54/17/33, 파트너전용 5/3/1, 라벨 개명 1/0/1, 곱도리 닉네임 1), 저점수 집합 동일. 속도 곱도리 119.9s→30.1s, 참 제육 122.0s→28.2s(navigate→Step 4 벽시계). dry-run T1~T4 재현, 실제 파일 md5 불변.
- **재현 실패 1건 — S2·S5 행 "숨김 상태 throttled 감지 유지":** 3회 중 2회 감지(2.69초·2.96초), 1회는 `throttled:false` 대신 가짜 `bottom:true`(hidden:true, 46/85, 4.55초). 기전: 숨김 직후 첫 라운드가 타이머 정렬로 0~1000ms 임의(544ms) → 3라운드째 `throttled` 검사 불성립 → 같은 라운드의 `stuck>=3 → wiggle → 바닥 판정`이 먼저 걸림(렌더링 정지로 `_atBottom()`·`|Δ|<50` 둘 다 참). 구 코드에는 바닥 판정이 없어 4라운드째 throttled가 떴을 경로.
- 부수 관찰: 숨김 중 렌더링 없이 전진한 구간이 호출당 5,400px(3×1800)로 가상 리스트의 위쪽 렌더 버퍼(4,487~5,138px)와 같은 크기 — 숨김 호출 1회 뒤 이어 붙이면 85/85 복구, 2회 연속(10,800px) 뒤에는 87/89(`누락 의심` 안전 실패).
- 기록 문구 오차 4건: 원칙 3 행 번호 38→41, META 추가분 열거, srcLen 13,767→16,509, 바닥 3.1초→1.6초.
- 검증 회차는 어제 세션의 `_audit_*`·`_bm_defs`를 Browser 1에서 찾지 못해(Browser 2는 연결 끊김) 같은 세션 A/B로 대체했다.

### 항목별 변경

| 항목 | 저장소 SKILL.md 변경(절·함수, 762행 기준) | 실측 |
|---|---|---|
| 1. 라운드 시작 hidden 게이트 | **Step 2 `_scroll`**(324행): `if (document.hidden) { throttled = true; reason = 'hidden'; break; }` — 매 라운드 `scrollBy` 전에 본다. 반환에 `reason`('hidden' / fallback은 'slow' / 정상 ''). 3라운드 연속 600ms 초과 검사(336행)는 그대로 두고 `reason = 'slow'`만 붙임. 설계 근거 26·27행, `_wait` 주석 306행 갱신 | 숨김 호출 7회(3매장·4회 시험) 전부 `throttled:true reason:'hidden'`, `reason:'slow'` 0회. 호출 도중 가려진 경우(H2b) 52라운드 뒤 다음 라운드 시작에서 반환(마지막 라운드 687ms, lastGainY = scrollY 93,600 → 블라인드 전진 0) |
| 2. 바닥 판정 가드 | **Step 2 `_scroll`**(349행): `if (a2 === b && !document.hidden && roundMs[roundMs.length - 1] <= 600 && window._atBottom() && Math.abs(document.body.scrollHeight - shB) < 50) { bottom = true; break; }` | 가짜 `bottom` 0회(숨김 7회). 가시 바닥 경로 정상: 김치찜 1.05초(3R·wiggle 1·마지막 76ms)/1.59초(258~262ms), 참 제육 1.07초/1.58초 — 모두 `bottom:true hidden:false` 연속 2회 |
| 3. `lastGainY` + 되감기 | **Step 2 `_scroll`**(318·346·351·355행): 호출 시작 scrollY로 초기화, gained>0인 라운드(wiggle 포함)마다 갱신, 반환에 포함. **Step 3-2**(452~458행): throttled 뒤 `hidden === false` 확인 → `window.scrollTo(0, <lastGainY>); JSON.stringify(await window._scroll(25000, <expectedTotal 또는 null>))`로 이어서 호출(코드 블록 신설) | 되감기 후 재개 4/4: 김치찜 170/170, 곱도리 6개월 518/518(호출 전 가림·호출 도중 가림 각 1회), 참 제육 94/94 — 누락 0. 검증 회차의 "2회 연속" 조건(H1·H2a·H2b 모두 2회 연속)에서도 0 |
| 문서 | 3-2 449행 종료 조건에 "`hidden: false`인 채로", 460행 bottom 정의에 가드 반영·"`hidden: false`일 때만, 연속 2회", 465행 "hidden===true인데 throttled===false … 예산이 끝난 경우뿐" → "hidden===true인 반환은 `throttled:true, reason:'hidden'`뿐이어야 한다"로 교체. 트러블슈팅 745행(`reason:'hidden'`)·746행(`reason:'slow'` fallback)·747행(`bottom:true`인데 `hidden:true`) 신설/갱신, 748·750행 문구 보강 | `grep "예산이 끝난 경우뿐"` 0건. `lastGainY` 10곳, `reason ... 'hidden'` 5곳(코드 324행 + 문서 4곳) |
| checklist v6 | "되돌리면 안 되는 것" throttled 행에 게이트·가드·되감기 추가, [실행 준비 조건] 숨김 계측에 `reason:'hidden'`·되감기·2회 연속 조건, 버전 줄 | — |

### 실측 — 가시 경로 A/B (2026-09-23 13:20~14:10 KST, 조회 기간 `2026. 8. 24 (월) ~ 2026. 9. 23`, 같은 세션에서 A = 검증 회차 코드(749행 `_scroll` 원문, `_scrollOld`로 이름만 변경) → B = 신 `_scroll`(762행), 나머지 정의는 동일)

| 매장 | expectedTotal | A: collected+blocked, 라운드·시간 | B: collected+blocked, 라운드·시간 | 집합 | 필드 diff(stars·date·nickname·menu·reviewText) | blocked | warns A/B | countMatch |
|---|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | 170 | 169+1, 80R·9.4s(wiggle 1) | 169+1, 80R·7.1s(wiggle 0, avgRoundMs 89) | 169 = 169 | 0/0/0/0/0 | 2026082401079865 동일(9/23에도 아직 차단) | []/[] | true |
| 곱도리 | 84 | 84+0, 38R·4.0s | 84+0, 38R·3.6s | 84 = 84 | 0 | 0 | []/[] | true |
| 참 제육 | 94 | 94+0, 43R·4.5s | 94+0, 42R·3.9s | 94 = 94 | 0 | 0 | []/[] | true |

- 검증 회차 값(170/85/89)과 다른 것은 30일 창이 이틀 밀린 것(8/22~9/21 → 8/24~9/23)뿐이며, 같은 세션 A/B로 대체했다. 3매장 `filterOk:true`, totalWaitMs 436/437/430, 2-B 재주입(곱도리·참 제육) 성공(`_bm_defs` localStorage 저장값 전체 17,833자(src만 17,411자) — 새 `_scroll` 포함; 검증 회차 2에서 라벨 정정).
- 저점수: 김치찜 0, 곱도리 0, **참 제육 2건**(2026092202482591 2점 9/22 지니헤어 `[일석이조] 갓성비 반반 제육 단품` `[파트너전용] 아들이 맛있다고해서 …` — 신규, 2026092001471500 2점 9/20). Step 5 미실행 — 엑셀 md5 `6dae739a…` 불변.
- 곱도리 6개월(518건)도 가시 상태에서 2회 호출로 완수(215R·25.1s + 47R·4.9s, 518 = 전체(518)).

### 실측 — 숨김 시험 (사용자가 크롬 창을 가림/꺼냄, 신 `_scroll`)

| # | 매장·조건 | 숨김 호출 | 반환 | 되감기 → 재개 |
|---|---|---|---|---|
| H1 | 김치찜 30일, 가시 62/170 수집 후 **호출 전** 가림 | 2회 연속 | 둘 다 `throttled:true reason:'hidden' rounds:0 elapsedMs:0`, scrollY 39,600 불변, `bottom:false` | `scrollTo(0, 39600)` → 169+1 = 170 (57R·5.4s), B 집합 대비 누락 0 |
| H2a | 곱도리 **6개월(518)**, 리셋 직후 호출 전 가림 | 2회 연속 | 둘 다 rounds 0 즉시 | `scrollTo(0, 0)` → 518/518 (253R·22.8s) |
| H2b | 곱도리 6개월(518), **호출 도중** 가림 | 1회차: 52R·5.5s 진행 뒤 다음 라운드 시작에서 반환(마지막 라운드 687ms, `lastGainY` 93,600 = scrollY) / 2회차: rounds 0 | `reason:'hidden'` ×2, 가짜 bottom 0 | `scrollTo(0, 93600)` → 518/518 (206R·18.5s), 누락 0 |
| H3 | 참 제육 30일, 가시 55/94 수집 후 호출 전 가림 | 1회 | rounds 0 즉시 | `scrollTo(0, 39600)` → 94/94 (20R·1.8s), 누락 0 |

- 숨김 호출 7회 전부 `throttled:true reason:'hidden'`, 가짜 `bottom` 0회, `reason:'slow'`(fallback) 0회. 검증 회차에서 87/89로 끝났던 "숨김 호출 2회 연속" 조건은 H1·H2a·H2b에서 누락 0.
- H2a의 `collected:11`은 숨김 상태에서 리셋+`scrollTo(0,0)`을 한 시험 절차의 잔상(렌더링이 멈춰 DOM에 직전 하단 카드가 남음)이며 스킬 흐름에는 없는 상황.

### 수정 회차 검사

- 저장소 `SKILL.md` 코드 블록 14개: javascript 6개(3-2 되감기 블록 신설) `node --check` 통과(플레이스홀더 치환·async 래핑), bash 4개 `bash -n`, 5-2 python heredoc `py_compile` 통과, 코드펜스 28개(짝수), UTF-8 정상, `name: baemin-review` 유지.
- 파일 전체 grep: `예산이 끝난 경우뿐` 0, 옛 `throttled = true; break; }`(reason 없는 형태) 0, `연속 2회` 문장 3곳 모두 `hidden: false` 조건 병기, `lastGainY` 10곳(코드·주석 5, 문서 5).
- 파일을 다시 열어 `_scroll` 315~360행, 3-2 449~465행, 트러블슈팅 745~750행, checklist 13·92·187행을 눈으로 확인했다.
- Browser 1 `self.baemin.com` localStorage: 이 세션의 `_audit_A/B_*` 6개·`_bm_defs` 삭제, 잔존 0. 지난 세션(9/20·9/21) 키는 세션 시작 시 없었다.

### 하지 않은 것

- SKILL.md 347·460행의 "실측 3.1초" 바닥 문구는 지시대로(기록만 정정) 그대로 두었다 — 이번 실측은 1.05~1.59초.
- `_saveDefs` 함수 목록·Step 4 판정·`_parse`·`_applyPeriod`는 무변경. 재패키징·재업로드 없음.

## 검증 회차 2 (2026-09-23 13:46~14:37 KST, 사용자 폴더 `baemin-review_검증2_2026-09-23.md`) — 수정 기록 2 대조

- 대상 커밋 e83c4a5(`SKILL.md` 762행, md5 `c6ee79422c8e2f8eddc7b3f7087c8ecc`). 범위는 수정 기록 2 항목만. 조회 기간 `2026. 8. 24 (월) ~ 2026. 9. 23`.
- **재현 실패 0건.** 불일치는 기록 라벨 1건뿐이었다: 수정 기록 2 가시 A/B 절의 "`_bm_defs` src 17,833자"는 localStorage 저장값 전체(`{"sid":…,"src":…}` JSON 문자열) 길이였고, src만 재면 17,411자(브라우저·node 동일). 동작과 무관(사용자 판정) → 이 커밋에서 라벨만 정정(위 수정 기록 2 가시 A/B 절, 442행). 파일 전체에서 17,833·17,411은 그 한 곳뿐이었다.
- 코드 3가지 변경(라운드 시작 hidden 게이트 324행·바닥 판정 가드 349행·`lastGainY` 반환 + 3-2 되감기) 정적 재현, 옛 문구(`예산이 끝난 경우뿐`, reason 없는 `throttled = true; break; }`) 0건, 코드 블록 14개 검사 재현.
- **가시 경로 3매장 A/B 집합 동일:** 김치찜 170(169+1), 곱도리 84, 참 제육 94 — A = 옛 코드(ace835b 637행 `_scroll`) / B = 신 `_scroll`(762행), 필드 차이 0, warns []/[], B는 `bottom:true hidden:false` 연속 2회로 종료. 저점수는 참 제육 2건(2026092202482591, 2026092001471500)으로 수정 기록 2와 같다.
- **숨김 시험 3회**(H1 호출 전 가림·2회 연속, H2 호출 도중 가림·2회 연속, H3 호출 도중 가림 — 참 제육 30일/6개월): 전부 `reason:'hidden'`, 가짜 `bottom` 0회, `reason:'slow'` 0회, `lastGainY`로 되감은 뒤 재개해 누락 0(94/94, 905/905, 905/905).
- 엑셀 md5 `6dae739a…`·mtime 불변(Step 5·dry-run 미실행).
- **패키징·재업로드 완료:** 사용자 판정 뒤 저장소 `SKILL.md`(`c6ee7942…`)로 `baemin-review.skill` 생성(zip 안 md5 동일), 사용자가 재업로드. 이번 기록 정정 세션(2026-09-23)에서 확인한 설치본(`/root/.claude/skills/synced/<uuid>_<uuid>/baemin-review/SKILL.md`)은 762행·md5 `526f660d79a2afcb1aeeeaea701474e3` — 저장소와 **frontmatter 2행 `name: "baemin-review"`의 따옴표만 다르고**(71,094B vs 71,092B) 3행 이후 본문 md5는 둘 다 `e87b787360396c4922b2a8c426d223fa`로 같다. 동기화 과정에서 name 값에 따옴표가 붙는 것으로 보이며 내용 차이는 없다.

## 기간 변경 회차 (2026-09-23 — 사용자 요청: 조회 기간 30일 → 3개월)

정기점검이 아니라 사용자가 직접 요청한 변경이다. 쿠팡 스킬과 같은 세션에서 함께 진행했고, 순서는 사용자가 고른 대로 "실측 → 수정 → 같은 세션 실제 실행 검증(3매장, 계정 전환 포함) → 패키징"이다. Step 5(엑셀 저장)는 하지 않았다.

### 사전 실측 (수정 전, 기간 필터만 적용해 전체(N) 읽기 — 스크롤 없음)

| 매장 | 30일(9/20) | 3개월 | 6개월 |
|---|---|---|---|
| 김치찜의 정석 | 174 | 702 | 1,430 |
| 곱도리 | 86 | 373 | 518 |
| 참 제육 | 90 | 349 (검증 회차에서 측정) | 915(9/20) |

**배민 "최근 3개월"은 달력이 아니라 92일 고정**: 9/23 적용 시 `2026. 6. 23 (화) ~ 2026. 9. 23`. 6개월은 183일(9/23 → 3/24, 9/9 → 3/10), 30일은 30일. 따라서 `_applyPeriod('최근 3개월', 92)` 로 기존 ±1일 판정을 그대로 쓴다 — 달력 개월 계산이나 허용오차 확대는 필요 없었다(수정 전에 "달력 기준이라 ±1일이 깨질 수 있다"고 본 것은 [추론]이었고 실측으로 기각).

### 항목별 변경 (SKILL.md, 커밋 대상)

| 항목 | 위치 | 변경 |
|---|---|---|
| 기간 | description·Step 2 헤딩·`_applyPeriod` 기본값(`'최근 3개월', 92`)·`_prepare` 호출·조용한 0건 원칙 3·`filterOk` 설명·재시도 절차 4곳·엑셀 현황 문구·최종 보고·트러블슈팅 `filterOk` 행 | 30일 → 3개월(92일). 엑셀 현황 문구에 "첫 실행에서 31~92일 전 저점수가 한꺼번에 신규" 명시. 트러블슈팅에 "다른 기간이면 62일/91일 차이" 추가 |
| 호출 횟수 안내 | 3-2 "최대 4회" 불릿·실측 기준 불릿 | 3개월 실측(김치찜 2회·나머지 1회) 반영 |
| **긴 결과 읽기(신규)** | Step 4 반환부·폴백 절·트러블슈팅 `[TRUNCATED]` 행·5-1 | Step 4 가 `window._out4` 에 보관하고 950자 이하일 때만 반환, 넘으면 `LEN:n`. 900자씩 `substring` 을 `browser_batch` 1회로 읽고 5-1 에 `JSON_OK` 파싱 확인 추가. **Blob + `get_page_text` 폴백 폐기** — 아래 발견 1 |
| **게시중단·삭제 카드(신규)** | `_parse` 차단 판정·3-2 `blocked` 불릿·Step 4 `blockedKinds`·트러블슈팅 행 | `'게시중단 요청으로 인해'`(temp) 외에 `'영구 게시중단'`(perm)·`'게시자가 삭제한 리뷰'`(deleted) 도 `_blocked` 로. `_blocked[no]` 값이 종류. Step 4 가 `blockedKinds` 를 함께 반환 — 아래 발견 2, 사용자 결정 |
| 점검표 | [의도된 동작] 조회 기간·`filterOk` 정의 행 | 3개월(92일 고정)로 갱신 |

### 발견 1 — Blob + `get_page_text` 폴백이 죽어 있었다 (도구 쪽 변화)

참 제육 3개월 저점수 8건 = Step 4 JSON 1,267자 → 잘림. SKILL.md 대로 Blob URL 로 탭을 이동시켜 `get_page_text` 를 부르니 `Can't interact with browser-internal or unparseable URLs` 로 거부됐고, 그 탭에서는 `javascript_tool` 도 같은 이유로 거부돼 **탭 이동 순간 `window._all` 과 함께 결과까지 잃었다**(재수집으로 복구). 2026-07-20(v15) 에 검증됐던 경로가 도구 갱신으로 막힌 것. 대체 경로 실측: `window._out4.substring(0,900)`·`(900,1800)` 을 `browser_batch` 로 읽으면 1,456자가 두 조각으로 온전히 오고, 1,100자 한 조각은 1,000자에서 `[TRUNCATED]`. 참 제육 실제 결과(1,267자)도 두 조각으로 온전히 읽어 이어 붙였다. 30일 시절엔 저점수가 0~1건이라 이 경로가 한 번도 실제로 필요하지 않았다.

### 발견 2 — 3개월에서 처음 나타난 카드 유형 2가지 (사용자 결정: 둘 다 제외)

참 제육 저점수 8건 중 3건만 진짜였고 4건은 `임시조치에 대한 작성자 응답이 없어 영구 게시중단 되었어요`(우리가 신고해 내린 리뷰의 30일 임시차단이 영구 차단으로 바뀐 것, "원문보기" 버튼만 있음), 1건은 `게시자가 삭제한 리뷰입니다.`(손님이 지움, 별점·사장님 답글은 남아 있음). 기존 판정은 `게시중단 요청으로 인해`(임시차단) 문구만 봐서 이 5건이 `menu:""`·`(텍스트 없음)` 행으로 엑셀에 들어갈 뻔했다. 7월 초 리뷰들이라 30일 창에서는 보이지 않던 유형. 사용자에게 (a) 둘 다 제외 (b) 영구만 제외 (c) 표시하고 포함 을 제시, **(a) 둘 다 제외** 선택. 수정 후 재수집: 참 제육 `collected 341 + blocked 8 = 349`, `blockedKinds {perm 4, deleted 4}`(deleted 4 중 3건은 4~5점이라 저점수 집합엔 영향 없음), 저점수 3건(2026092202482591·2026092001471500·2026062600359205), `dist [0,2,1,7,331]`, parseFail 0.

주의: 배민 "차단" 탭 숫자와 대조할 때 `blocked` 전체가 아니라 temp+perm 만 같아야 한다(deleted 는 차단 탭에 없음) — 트러블슈팅 행에 적었다.

### 실제 실행 (수정본 2-A/2-B 원문, 3개월, 09:14~09:35 UTC, 창 가시 상태)

| 매장 | 계정 | expectedTotal | collected + blocked | `_scroll` 호출 | 라운드·소요 | avgRoundMs | 저점수 | parseFail·warn | 매장 벽시계(Step 2 시작→Step 4) |
|---|---|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | A | 702 | 701 + 1 = 702 ✓ | 2 | 236R·25.0s(+468) → 115R·11.5s(+227) | 99·100 | 1 (3점 7/16) | 0·0 | 67.4s |
| 곱도리 | A | 373 | 373 + 0 = 373 ✓ | 1 | 189R·20.5s | 100 | 2 | 0·0 | 39.9s |
| 참 제육(수정 전 `_parse`) | B | 349 | 349 + 0 = 349 ✓ | 1 | 168R·18.0s | 98 | 8 (perm 4·deleted 1 포함) | 0·0 | 34.9s |
| 참 제육(수정 후 `_parse`, 같은 탭 재수집) | B | 349 | 341 + 8 = 349 ✓ | 1 | 164R·15.1s | 92 | 3 | 0·0 | — |

- 3매장 `filterOk true`, range `2026. 6. 23 (화) ~ 2026. 9. 23`, 수집된 날짜 최소/최대 = 6/23 ~ 9/23(범위 밖 0건), `hidden false`, throttled 0회.
- 처리율 약 20건/초(30일 실측 22~25건/초와 같은 수준). 25초 예산 1회가 약 470건 — 김치찜(702)만 2회.
- 2-B 재주입: 곱도리·참 제육 모두 `_bm_defs`(SID `20260923T0910`) 로 정상. 계정 A→B 전환 뒤에도 같은 오리진 `localStorage` 가 남아 재주입됐다.
- 벽시계는 도구 왕복 포함 값이며 사용자 대기(계정 전환)는 제외.

### 수정 회차 검사

코드펜스 28(짝수), UTF-8 정상, `name: baemin-review` 유지, javascript 블록 6개 전부 `node --check` 통과(자리표시자 `<…>` 를 `null` 로 치환해 검사; 설치본도 같은 3개 블록이 자리표시자 때문에 원문 그대로는 실패 — 신규 실패 아님), 잔존 "30일" 은 전부 이력·후보 목록·주석. python 문자열 치환 13 + 3 + 4 + 2 + 2건 각 1회 매칭 확인 후 적용.

### 하지 않은 것

- Step 5(엑셀 저장) 미실행 — 첫 3개월 실행에서 31~92일 전 저점수(오늘 기준 김치찜 1·곱도리 2·참 제육 3 중 기존 엑셀에 없는 것)가 신규로 들어가는 것은 사용자가 알고 있다.
- 별도 세션 검증 회차 — 사용자 판단. 이번 변경은 사용자 요청 회차라 같은 세션에서 3매장 실제 실행으로 갈음했다.
- 설치본 재업로드(사용자 몫 — 패키징까지만).

## 다음 점검에서 대조할 것

- (기간 변경 회차 추가) 3개월 첫 실제 실행: `blockedKinds` 가 보고되는지, 참 제육 `blocked` 8(perm 4·deleted 4) 근처인지와 "차단" 탭 숫자 = temp+perm 인지, 저점수 신규 건수, Step 4 `LEN:n` 경로가 실제로 쓰이고 5-1 `JSON_OK` 가 찍히는지.
- (기간 변경 회차 추가) 김치찜 게시중단 2026082401079865 의 30일 해제 후 상태 — 3개월 창에서는 해제되면 정상 수집 대상(temp → 사라짐)이고, 영구 차단되면 perm 으로 남는다.
- (기간 변경 회차 추가) 도구 쪽 `blob:` URL 거부가 유지되는지(되돌아와도 Blob 폴백을 되살리지 않는다 — 조각 읽기가 탭을 옮기지 않아 더 안전).

- 사용자가 고른 결함·개선안·속도 항목의 수정 반영 여부(수정 회차 기록 참조) — 특히 결함 1(엄격 필터 판정)이 3매장 정상 경로에서 `pass:true` 인지, 결함 3 수정 후 파트너전용 9건의 reviewText 가 본문 1회만 담는지.
- S2·S5 채택 시 **숨김 상태에서 `throttled` 감지가 유지되는지**(적응형 대기·큰 스텝으로는 미실측), 그리고 4-2 375행 "최대 8회" 상한·363/370행 종료 문구의 정합.
  → (수정 회차 2) 검증 회차 3회 중 1회 미감지(가짜 bottom) → 라운드 시작 hidden 게이트·바닥 판정 가드·`lastGainY` 되감기로 수정, 숨김 호출 7회 전부 감지·재개 누락 0. **다음 검증 회차가 볼 것:** 숨김 시험에서 `reason:'hidden'`·가짜 bottom 0·되감기 후 `collected+blocked === expectedTotal`, 호출 도중 가림 1회 포함; `reason:'slow'`(fallback)가 실제로 뜨는 경우가 있는지.
- S1 채택 시 scrollHeight 미세 변동으로 첫 wiggle 이 바닥 판정을 놓치는 빈도(오늘 1/2).
- 게시중단 2026082401079865 의 30일 차단 해제(9/23 경) 뒤 김치찜 `blocked` 0·countMatch 유지 여부, 그리고 그 리뷰의 별점.
  → (수정 회차 2) 9/23 14시 KST에도 아직 차단(blocked 1, 169+1=170). 다음 회차에 재확인.
- 쿠팡 스킬: Step 0 한 줄 판정(쿠팡 SKILL.md 39행)이 배민 결함 3 과 같은 문제인지 쿠팡 회차에서 판정할 것(이번 회차 [추론]).
- `expectedTotal === null` 강제 시 흐름(3회차 이월).
- 새로 달린 참 제육 2점 리뷰(2026092001471500)가 다음 실행에서 엑셀에 신규로 들어가는지(이번엔 Step 5 미실행).
  → (수정 회차 2) 참 제육에 2점 리뷰가 1건 더 달렸다(2026092202482591, 9/22, 파트너전용). 두 건 모두 다음 실제 실행에서 신규로 들어가야 한다(이번에도 Step 5 미실행).
- F 실험 결과에 따라 45초 CDP 타임아웃 후보(intensive throttling)의 채택·기각.
- (수정 회차 추가) 설치본 md5가 저장소 `SKILL.md`(수정 회차 버전, 749행)와 같은지 — 다르면 "재업로드가 안 된 것". 사용자가 검증 세션 뒤 재패키징·재업로드하기로 함.
  → (수정 회차 2) 기준은 이제 762행 버전(이 커밋). 검증 회차 2를 통과한 뒤 재패키징·재업로드.
- (수정 회차 추가) 남은 도구 오버헤드의 대부분인 Step 4 블록(약 2KB) 생성 16~24초 — 정의에 `_extract(ET, FILTER_OK, AUTH, name)` 함수로 넣고 한 줄 호출로 바꾸는 후보(미채택).
- (수정 회차 추가) `localStorage`에 남긴 `_audit_A_*`·`_audit_B_*`는 검증 회차가 대조 후 지울 것. `_bm_defs`는 두어도 된다(SID 불일치로 재사용 안 됨).
- (검증 회차 2 추가) **숨김 호출이 2회 연속이면 두 번째 반환의 `lastGainY`가 실제 마지막 수집 지점보다 1,800px 아래다**(검증 회차 2 H2: 1회차 lastGainY 90,000 / 2회차 91,800 = 그 호출 시작 scrollY). `_scroll`이 `lastGainY`를 호출 시작 시점의 scrollY로 초기화하기 때문이다. 블라인드 전진 1라운드분이라 렌더 버퍼(4,487~5,138px) 안이어서 누락은 0이었다. **다음 수정 회차 후보:** `_scroll` 시작 시 `lastGainY`를 현재 scrollY가 아니라 직전 호출 값(`window._lastGainY`)으로 초기화하고, 반환 때 `window._lastGainY`에 저장하는 안(매장 전환 시 `window` 리셋으로 자동 초기화). 미채택·미수정.
- (검증 회차 2 추가) 설치본 md5가 저장소 `SKILL.md`(`c6ee7942…`, 762행)와 같은지. 다르면 먼저 frontmatter `name` 따옴표 차이인지 확인할 것 — 2026-09-23 설치본은 `526f660d…`로, 2행 `name: "baemin-review"` 따옴표만 다르고 3행 이후 본문 md5(`e87b7873…`)는 같았다. 본문까지 다르면 재업로드가 안 된 것.

# 수정 회차에 적용할 것 (점검표에서 옮김 — 다음 단계용)

- **수정은 저장소 `SKILL.md`에서 한다.** 설치본을 직접 고치면 세션이 끝나며 사라진다. 저장소에 push한 뒤 그것으로 패키징해라.
- **한 번의 수정 → 한 번의 패키징 → 한 번의 재설치.** 중간에 다른 세션을 열면 그 세션은 옛 캐시를 읽고 그 위에 수정한다. 먼저 한 수정이 조용히 사라진다.
- **수정 후 반드시 파일을 다시 열어 눈으로 확인하고 나서 "완료"라고 말해라.**
- **고친 뒤 실제 사이트에서 1회 실행해라.** 문서만 고치고 끝내지 마라. 속도 항목은 이번 기준선의 A/B 합격 기준(집합·필드·warns·blocked·parseFail 동일)으로 3매장 재확인.
- **같은 개념을 두 곳에서 고칠 때는 기준을 대조해라.** 종료 조건(336·363·370·371행), hidden(26·126·334·366·627~629행), 잘림 표기(419행·트러블슈팅)는 같은 규칙이 여러 곳에 있다. **문구 교체는 파일 전체 검색으로**(점검표 개정안 6).
- **판정 조건을 완화하는 수정을 했으면 원래 잡히던 실패가 여전히 잡히는지 다시 확인해라.** 결함 1 의 엄격 판정은 완화가 아니라 강화지만, 정상 경로 3매장이 통과하는지 반드시 확인.
- **"되돌리면 안 되는 것" 표에 있는 것을 건드렸으면 그 표도 같이 갱신해라.** S1·S2·S5 는 `_scroll`·`throttled` 행과 접한다.
- **수정과 검증은 다른 세션에서 한다.**
- 작업 경로 세 단계: 저장소 `SKILL.md` 수정 → push / `.skill` 재패키징 / **사용자가 Claude 설정에서 재업로드**. 재업로드 전에는 실행에 반영되지 않는다.

---

# 이전 기록 (2026-09-09 회차 — 원문 보존)

# 점검 기준선

점검일: 2026-09-09
결함 4건 / 개선안 5건 / 인용불가·미입증으로 제외 2건
직전 기준선 대비: 기준선 파일 없음(신규 개설). 인계 문서(사용자 폴더 `배민리뷰_스킬점검_인계_20260909.md`)의 지적 A~D와 미확정 2건을 입력물로 판정: 해결 0건, 미해결 3건(A·B·C), 근거없음 1건(D), 미확정 판정 완료 2건(1번·2번), 신규 3건(결함 2·3·4 — 결함 1은 [A]의 승격)
점검 대상: 저장소 `SKILL.md` 599행 (md5 `499afc0758fcb42fc481d4e4da0d472a`, 설치본과 동일) /
          실제 실행: 3매장 전부 Step 0~4-3까지(김치찜의 정석·퍽퍽살이 싫어 내가 만든 곱도리·참 제육), Step 5 미실행 /
          엑셀 백업: `$HOME/skillwork/배민_백업_20260909_0404.xlsx` (원본은 헤더 1행만, 데이터 0행) /
          브라우저: Claude in Chrome, 계정A→계정B 전환은 사용자가 수행. 점검 모델: Fable(진단 전용 세션)
정정 회차(2026-09-09, 검증 세션의 지적 반영): 검증 결과 **코드·동작 재현 실패 0건**, 대신 기록 문구 4건이 틀려 `audit/` 두 파일만 고침. `SKILL.md`는 손대지 않음. 상세는 아래 "정정 기록" 절.
교차 정정 회차(2026-09-09, 쿠팡 스킬 점검 개정안 이관): 백업 경로 2건 + README 설치본 경로 1건. `SKILL.md` 판정 로직은 손대지 않음. 상세는 아래 "교차 정정 기록" 절.
1차 커밋(진단): `audit/last-audit.md`만 추가. 2차 커밋(수정, 같은 날 같은 세션 — 사용자 지시): 사용자가 고른 결함 1·2·3·4 + 개선안 ①·② 를 저장소 `SKILL.md`에 반영하고, 점검표 개정안 10건 + "PC에서 push" 절차를 `audit/checklist.md`에 반영, 이 파일에 "수정 기록" 절 추가. **재패키징·재업로드는 하지 않았다** — 사용자가 다른 세션에서 검증을 받은 뒤 직접 한다. 그 전까지 설치본은 진단 시점 버전(md5 `499afc0758fcb42fc481d4e4da0d472a`)이다.

## 시작 전 확인 결과

- 토큰: 요청 문구에 포함되어 있었음. **작업이 끝나면 폐기할 것**(대화에 남음).
- 정본 대조: 저장소 `SKILL.md` = 설치본 (md5 동일, 599행 / 39,011 bytes). 단 점검표가 적은 설치 경로 `/mnt/skills/plugins/baemin-review/SKILL.md`는 이 환경에 **존재하지 않음**. 실제 설치본은 스킬 호출 헤더가 알려주는 `/root/.claude/skills/synced/<uuid>_<uuid>/baemin-review/SKILL.md`였다.
- 기준선: `audit/last-audit.md` 없음 → 신규 진단. 대신 사용자 폴더의 인계 문서를 입력물로 썼다.

## 직전 입력물(인계 문서) 판정

| 항목 | 판정 | 근거(지금 원문·실측) |
|---|---|---|
| 미확정 1. 스크롤 감속·정지 원인 (rAF vs setTimeout) | **판정 완료 — 둘 다 맞고 역할이 다르다** | [실측] 탭 생성 직후 `document.hidden === true`. `setTimeout(250)` 실측 741/994/1007/994/1003ms → **1초 클램프 = 감속 원인**. 같은 상태에서 `requestAnimationFrame`은 2000ms 안에 한 번도 안 옴 → **렌더링 정지 = 완전 정지 원인**(무한스크롤 로더가 안 돎). 숨김 상태 `_scroll(20000)`: 12라운드·평균 1000ms·gained 0·scrollHeight 4362 고정. `computer scroll` 1회 후 카드 6→15, scrollHeight 4362→11224(프레임 1장 강제 렌더). 창을 전면으로 꺼낸 뒤 hidden false, 타이머 251~261ms, 95라운드/25.3초/+92건. SKILL.md:592 원문 `탭이 백그라운드라 rAF 스로틀링` 은 정지는 설명하나 감속은 설명 못 함. 45초 CDP 타임아웃(직전 세션)은 1초 클램프만으로 설명 안 됨 → 아래 "미확정"에 남김 |
| 미확정 2. 수집 187 vs 전체 188 | **판정 완료 — 게시중단 리뷰 차이** | [실측] 오늘 김치찜: `전체(190)`, 수집 189, `_parse`가 `게시중단 요청으로 인해`로 건너뛴 카드 1건(리뷰번호 2026082401079865), 배민 탭 `차단(1)`. **189 + 1 = 190 정확히 일치.** 곱도리·참제육은 96/96·`차단(0)`. 900px 스텝에서 누락 없음 → "굵은 OS 스크롤 누락" 가설은 오늘 근거 없음. 어제의 187/188 자체는 소급 확인 불가하나 같은 기전(게시중단 1건, 8/24자)으로 설명됨 |
| [A] hidden 감지 후 사후 대응만 (SKILL.md:121, 346) | **미해결** | 원문 그대로: `- \`hidden === true\` → 이 값을 기억해 둔다. Step 4의 스크롤이 정체될 때만 쓴다.` / `다음 호출 직전에 \`computer(action="screenshot", tabId=<탭ID>)\`를 한 번 끼워 넣어 렌더링을 재개시킨다.` 추가로 [실측] 스크린샷은 hidden을 false로 바꾸지 못하고(스크린샷 직후 hidden true, 타이머 911/999/1001ms) 프레임 1장만 강제한다. 즉 [A]를 문구대로 반영해도 "호출당 한 배치"밖에 안 나온다 → 결함 1로 승격 |
| [B] `_scroll`이 감속을 감지 못함 (SKILL.md:303, 311) | **미해결** | 원문 그대로: `window._scroll = async function(budgetMs = 25000, target = null) {` … `await new Promise(r => setTimeout(r, 250));` [실측] 숨김 상태에서 12라운드×1000ms·gained 0으로 예산 20초를 끝까지 소진 → 개선안 1 |
| [C] 굵은 OS 스크롤 금지 명문화 | **미해결(문서에 없음)** | SKILL.md에 해당 문구 없음. [실측] 900px 스텝은 3매장 모두 누락 0. 결함 1 수정 시 함께 문서화 권고 |
| [D] `computer wait` 가 hidden을 false로 바꿈 | **근거없음** | [실측] `computer(wait, 1s)` 직후 hidden true 유지, 타이머 535/1000/1001/999/1016ms. hidden이 false가 된 시점은 사용자가 크롬 창을 전면으로 꺼낸 순간(04:10:53Z, 탭 생성 후 346초). 채택하지 말 것 |

## 결함

| # | 심각도 | 줄 | 문제 원문(그대로) | 실측/추론 | 왜 틀렸는지 | 수정 방향 |
|---|---|---|---|---|---|---|
| 1 | 상 | 26, 121, 346, 592 | 26: `이 환경에서 탭은 \`document.hidden === false\`이고 rAF 스로틀링이 없다.` / 346: `\`computer(action="screenshot", tabId=<탭ID>)\`를 한 번 끼워 넣어 렌더링을 재개시킨다.` / 592: `탭이 백그라운드라 rAF 스로틀링` | [실측] | 지금 이미 틀림. `tabs_context_mcp(createIfEmpty=true)`로 만든 탭이 04:05:07Z~04:10:5xZ 동안 `hidden: true`(창이 가려짐/비활성). 이 상태에서 (a) setTimeout 1초 클램프, (b) rAF 미발생으로 리스트가 더 로드되지 않아 gained 0. 문서가 지시하는 스크린샷·wait 모두 hidden을 해제하지 못함(프레임 1장만). 결과: `gained===0` 연속 2회로 종료 → 수집 6/190 → `수집률 낮음` ok:false. 엑셀은 보존되지만 **실행 자체가 실패**한다 | Step 2에서 `hidden === true`면 Step 4로 가지 말고 사용자에게 "크롬 창을 앞으로 꺼내 달라" 요청 후 hidden false를 확인하고 진행. 26·121·346·592 문구를 실측대로 고침(감속=타이머 클램프, 정지=렌더링 중단, 해제는 창 가시화뿐). [C] "굵은 OS 스크롤 대체 금지"도 같이 명문화 |
| 2 | 중 | 221, 586 | 221: `// 별점: aria-label이 가장 정확하다(실측 100%). SVG 색상 세기는 44같은 이상값을 만든 전력이 있어 fallback으로만 쓴다.` / 586: `\`aria-label\` 우선 경로가 실패한 경우다. 카드 구조를 다시 확인할 것` | [실측] | 지금 이미 틀림. 3매장 381건(189+96+96) 전부 카드 안에 `aria-label` 요소가 0개. 별점은 **전건 SVG 색상 fallback**(`fill="#FFC600"`, 16px svg 5개)으로 읽혔다. 결과는 맞았음(parseFail 0, 분포 김치찜 0/0/0/1/188·곱도리 0/0/0/1/95·참제육 0/0/0/5/91, 4점 카드 본문 확인). 그러나 문서의 1순위 경로는 죽어 있고, 586의 대응 문구는 정상 상태를 "실패"로 오인하게 한다 | 221·586을 실측대로 갱신(현재 유효 경로 = SVG 색상, aria-label은 대비용). 색상 경로에 `svg[width="16"]` 같은 크기 조건을 두어 44 재발 방지. 점검표 "되돌리면 안 되는 것"의 `별점 aria-label 우선` 행도 재검토 |
| 3 | 하 | 49 | `ls -d $HOME/mnt/claude && python3 -c "import openpyxl;print('openpyxl ok')" && ls $HOME/mnt/claude/'~$배민_저점수리뷰.xlsx' 2>/dev/null && echo LOCKED \|\| echo UNLOCKED` | [실측] | 특정 조건에서 반드시 틀림(조건 재현함): 폴더가 없거나 openpyxl이 없으면 `A && B && C && echo LOCKED \|\| echo UNLOCKED` 우선순위 때문에 **`UNLOCKED`가 찍힌다**(재현: `ls -d $HOME/mnt/NOPE && … \|\| echo UNLOCKED` → stderr 오류 + `UNLOCKED`). 52~53행 판정은 LOCKED/device_bash 실패만 다루므로 "폴더 없음"이 정상으로 통과한다 | 세 검사를 분리해 각각 `FOLDER_OK/NO_FOLDER`, `OPENPYXL_OK/NO_OPENPYXL`, `LOCKED/UNLOCKED`를 따로 출력 |
| 4 | 하 | 199 | `배민 리뷰 목록은 virtual scroll이라 DOM에는 화면에 보이는 약 6개만 유지된다.` | [실측] | 낡은 수치. 최상단에서는 6개지만 scrollY 3451에서 DOM 카드 15개(파싱 누적 17 > 15 이므로 윈도잉 자체는 존재). 동작에는 영향 없음 | "약 6~15개" 또는 "화면 주변만 유지된다"로 문구 정정 |

인용불가·미입증으로 제외한 항목 2건: (1) SKILL.md:301 `_lost`의 로그인 URL 패턴 `/login|signin|auth/i`가 실제 배민 로그인 리다이렉트 URL과 맞는지 — 로그아웃 조건을 만들 수 없어 미입증[추론]. (2) SKILL.md:259 닉네임 3차 fallback `lines.findIndex(l => l.includes(dm[1]))`가 연도(2026)를 포함한 다른 줄을 먼저 잡을 가능성 — 실측 3건(픽업 카드)에서 모두 정답이라 조건을 만들지 못함.

## 개선안 (최대 5)

| # | 내용 | 이유 | 우선순위 |
|---|---|---|---|
| ① | `_scroll` 자기 감속 감지 (`throttled`) | **2026-09-09 수정 회차에 반영됨.** 실측: 가린 창에서 라운드 998/1001/1001ms → 조기 반환, 꺼낸 뒤 257ms·+54건/12초 복구. **판정 기준은 시간이 아니라 `3라운드 연속 600ms 초과`다** — 반환까지의 시간은 3.1~5.5초로 변한다(아래 수정 기록 참조) | 완료 |
| ② | 게시중단 건수 `blocked` 집계·`countMatch` 보고 | **2026-09-09 수정 회차에 반영됨.** `ok` 판정에는 넣지 않음(다음 회차 결정) | 완료 |
| ③ | Step 3 적용 후 다이얼로그 닫힘 + `range`가 오늘 기준 30일 범위와 일치하는지 코드로 판정 | [실측] 다이얼로그가 열린 동안에도 `labelOk` true·range 정규식은 6개월 범위를 잡음. 적용 클릭 무효 조건은 만들지 못해 개선안 | 1 (이월) |
| ④ | 사이트 현행화: (a) `DT`에 `'픽업'` (b) Step 2 `titleOk`를 헤더 줄로 한정 (c) 하단 프로모션 배너(`FloatingBanner-module`, fixed z-index 101, role 없음, `aria-label="닫기"` 버튼 있음)를 `_dismiss`가 닫도록 확장 — JS `.click()` 경로는 영향 없으나 뷰포트 하단 124px를 가리고, 뷰포트 높이 약 730px 이하에서는 좌표 클릭 fallback의 `적용` 버튼과 겹칠 수 있음[추론] | (a)(b) 1회차 실측. (c) 2026-09-09 수정 회차 실측: 기간 버튼(y 532~596)·다이얼로그(y 156~699)·적용(y 631~679)과 배너(y 715~839, innerHeight 855) 겹침 없음, `elementFromPoint` 모두 본래 요소 → 결함 아님 | 2 (이월) |
| ⑤ | 검증 공백: 실행 전 자동 백업을 Step 0에, Step 5 저장 후 파일 재열기 확인 | 스냅샷 방식이라 백업이 사실상 필수인데 절차에 없음 | 3 (이월) |

## 실행 실측 기록

조회 기간(3매장 공통): `2026. 8. 10 (월) ~ 2026. 9. 9 (수)` (최근 30일). 기본 필터는 `최근 6개월`이며 6개월 총건수는 김치찜 `전체(1,453)`(쉼표 실재)·곱도리 485·참제육 940.

| 매장 | expectedTotal | collected | 게시중단(차단 탭) | 별점분포 1~5 | parseFail | _scroll 호출 | 라운드·소요 | hidden |
|---|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | 190 | 189 | 1 / `차단(1)` | 0/0/0/1/188 | 0 | 숨김 1회 + 가시 3회 | 숨김: 12R·20.2s·+0 / 가시: 95R·25.3s·+92 → 91R·25.1s·+80 → 48R·25.3s·+0(종료 확인, 하단 도달) | 04:05:07Z 탭 생성부터 true, 04:10:53Z(346초) 사용자 조치 후 false |
| 퍽퍽살이 싫어 내가 만든 곱도리 | 96 | 96 | 0 / `차단(0)` | 0/0/0/1/95 | 0 | 2회 | 90R·23.2s·+89 → 1R·0.3s·+1(target 도달) | false |
| 참 제육 | 96 | 96 | 0 / `차단(0)` | 0/0/0/5/91 | 0 | 1회 | 86R·22.1s·+90(target 도달) | false |

- 라운드당 소요(가시): 평균 257~259ms, 최대 267ms. 숨김: 평균 ~1000ms. 처리율 ≈ 3.7건/초 → SKILL.md:340 `205건이 약 50초(호출 2회)` 는 여전히 대체로 유효(190건 = 2회 50.4초 + 종료확인 1회).
- `setTimeout(250)` 실측(ms): 숨김 741/994/1007/994/1003 · wait 후 535/1000/1001/999/1016 · 스크린샷 후 911/999/1001/1000/998 · 가시 261/254/257/260/251/257/260/251. rAF: 숨김에서 2회 모두 >2000ms 미발생.
- Step 2: 3매장 모두 `titleAt: 0ms`(navigate 직후 매장명 즉시 존재), 팝업은 참제육(계정B)에서 `dont_show` 1회 닫힘, 김치찜·곱도리 `none`. 하단 `기상악화 시…` 배너는 role=dialog가 아니라 `_dismiss` 대상이 아니며 필터 조작을 방해하지 않았다.
- Step 3: 기간 버튼 = `BUTTON: 최근 6개월|2026. 3. 10 (화) ~ 2026. 9. 9 (수)` 1개, 라디오 5개(`name="review-date-filter"`, `label[for]` 구조 유지) + 라벨 없는 라디오 1개(`:r12:`). `적용` 후보 버튼은 다이얼로그 안 1개뿐. 적용 클릭 후 `전체(N)` 갱신 시점: 김치찜 1,453→190 @959ms(숨김), 곱도리 485→**null@252ms**→96@516ms, 참제육 940→96@252ms. 2.5초 대기는 3회 모두 충분.
- 별점 소스: 381/381 `svgcolor`(aria-label 0건). 카드 내 svg = 별 5개(16px, `#FFC600` 또는 회색) + 12px 아이콘 1개(`rgb(24,26,28)`). 
- 닉네임 소스: `dt` 377 / `year` 3(픽업 카드) / 실패 1(곱도리 2026082400693614, DOM에서 회수되어 원인 미확인).
- 배달유형 첫 줄 분포: 알뜰배달 321 / 한집배달 49 / 가게배달 8 / **픽업 3**. `PK` 키워드 신규 없음(파트너 전용 리뷰 0건).
- 헤더 계정명은 계정A·B 모두 `한종원님` (SKILL.md:123 주의 문구 유효).
- 저점수(1~3점) 0건, 신규 0건. 엑셀은 헤더만(0행) — Step 5 미실행.

## 미확정으로 남긴 것

- **직전 세션의 45초 CDP 타임아웃 기전.** 1초 클램프만으로는 25초 예산 루프가 ~26초에 끝나야 하므로 설명이 안 된다. 후보는 Chrome intensive throttling(숨김 5분 이후 연쇄 타이머 분당 1회). 오늘은 탭 숨김 256초 시점에 연쇄 8회가 1207/1002/990/1010/991/1008/994/1001ms였고, 346초 시점에 사용자가 창을 꺼내 hidden이 false가 되어 5분 이후 상태를 측정하지 못함. 추정을 결론으로 적지 말 것.
- **왜 첫 탭이 hidden으로 시작하는가.** `createIfEmpty`로 새 창이 생기며 그 창이 다른 창(Claude 앱으로 추정)에 가려진 것으로 보이나, 어느 창이 가렸는지는 사용자 화면을 보지 못해 확인 못 함.
- `_lost`의 로그인 URL 패턴 실효성([추론]).

## 점검표 개정안

1. 시작 전 확인 ②의 설치본 경로 `/mnt/skills/plugins/baemin-review/SKILL.md` → 이 환경에는 없다. "스킬 호출 헤더의 `Base directory for this skill:` 경로"로 바꿀 것(이번엔 `/root/.claude/skills/synced/…/baemin-review/SKILL.md`).
2. 시작 전 확인 ③의 백업 명령에 `mkdir -p "$HOME/skillwork" &&` 를 앞에 붙일 것 — 디렉터리가 없어 그대로 실행하면 실패한다(이번 세션에서 없었음).
3. "이번 회차에 반드시 판정할 것" 절은 저장소 밖 파일(사용자 폴더의 인계 문서)을 전제로 한다. 인계 문서를 `audit/` 아래에 두거나, 이번 기준선에 흡수했으니 이 절을 삭제하고 "직전 기준선 판정"으로 일원화할 것.
4. 인계 A~D·미확정 1~2는 이번에 판정했으므로 다음 회차 점검표에서 빼고, 대신 "다음 점검에서 대조할 것"을 읽게 할 것.
5. "되돌리면 안 되는 것" 표의 `별점 aria-label 우선` 행: 전제(aria-label 존재)가 현재 사이트에 없다(결함 2). 행을 "별점 색상 경로에 svg 크기 조건·1~5 범위 가드 유지"로 바꿀지 사용자 결정 필요.
6. 마무리 절차의 모순: "① push → ② 재clone → ③ 채팅 보고"와 "checklist.md는 채택 여부를 묻기 전에 고치지 마라"·"둘 중 하나만 push하면 어긋난다"가 동시에 성립할 수 없다. 진단 회차는 `last-audit.md`만 1차 커밋, 채택 후 `checklist.md` 2차 커밋으로 절차를 명시할 것(이번에 그렇게 함).
7. 실행 준비 조건에 "크롬 창이 다른 창에 가려지지 않게 전면에 둘 것"을 추가하고, 표준 계측으로 "탭 생성 직후 `document.hidden`·`setTimeout(250)` 실측"을 고정할 것. 이번 회차에서 이 계측이 결정적이었다.
8. 5분 intensive throttling 판정은 탭을 5분 이상 숨긴 채 유지해야 해서 정상 실행 계측과 양립하지 않는다. 별도 실험 항목으로 분리하거나 삭제.
9. 매 회차 통과만 나온 항목: "매장 정보 표(shopId 3개·계정 A/B)" — 3회째 그대로 통과. 유지하되 "실행 시 자동 확인됨"으로 격하.
10. 사용자 폴더의 `baemin-review.skill`(7/26, 39,036 bytes)은 9/6 개정 이전 패키지로 보인다. 점검표에 "폴더의 .skill 파일이 최신 패키지인지"를 확인 항목으로 넣을지 결정 필요(이번엔 확인 안 함).

## 수정 기록 (2026-09-09 수정 회차)

사용자 지시로 진단 세션이 이어서 수정함(검증은 별도 세션). 대상: 결함 1·2·3·4, 개선안 ①·②, 결함 5(신규 지시) 판정, 점검표 개정안 10건 + PC push 절차.

| 항목 | 저장소 SKILL.md 변경 | 실측 |
|---|---|---|
| 결함 1 (a) 설계 근거 | **`## 설계 근거` 2번째 불릿**(636행 기준 26행 — 599행 기준에서도 26행, 우연히 같음) 단락 교체: "hidden === false·rAF 스로틀링 없음" 단언 삭제 → 새 탭이 hidden: true로 시작할 수 있음, 감속(setTimeout 클램프 741/994/1007/994/1003ms)·정지(rAF 2초 내 0회, 12라운드 gained 0) 두 증상과 원인, 스크린샷·wait 무효, 해제는 창 가시화뿐, 가시 상태 257ms/라운드 | 진단 회차 실측 |
| 결함 1 (b) Step 2 | `hidden === true` → Step 3으로 가지 않고 사용자에게 요청 후 대기. 문구 고정: "⚠️ 크롬 창이 다른 창에 가려져 있어(document.hidden) 리뷰 목록이 로드되지 않습니다. 크롬 창을 화면 앞으로 꺼내 주시고(최소화 해제, Claude 앱에 가려지지 않게) '꺼냈어요'라고 알려주세요." 최대 2회, 그래도 true면 `ok:false, reason:"탭 숨김 — 크롬 창이 가려져 수집 불가"` | 수정 회차 실측: 새 탭 2/2 hidden true로 시작, 타이머 999/1001/1000/1000ms |
| 결함 1 (b) Step 4-2 | "hidden이면 스크린샷 1회" 삭제 → `throttled === true`면 즉시 멈추고 같은 문구로 요청, 응답 후 같은 `_scroll` 이어서 호출(`window._all` 유지). `throttled`인 `gained 0`은 종료 근거 아님, 8회 상한에 미포함 | 수정 회차 실측: 꺼낸 직후 호출 257ms·+54건 |
| 결함 1 (c) 트러블슈팅 | **`## 트러블슈팅` 표의 `스크롤이 정체되고 hidden: true` 행**(599행 기준 592행) 1행 → 3행(감속 / 정지 / Step 2 hidden)으로 분리, 원인·대응을 실측대로. **636행 기준 626·627·628행** | — |
| 결함 1 (d) = 개선안 ① | `_scroll`에 라운드 실측 `roundMs` 기록, **3라운드 연속 600ms 초과면 `throttled: true`로 즉시 반환**. 임계 근거 주석: 정상 251~267ms(평균 257)의 2배 이상이면서 클램프 1000ms의 60%; 첫 라운드 콜드 스타트 480~741ms 때문에 3연속 요구. 반환값에 `throttled`·`avgRoundMs`·`lastRoundsMs`·`blocked` 추가 | **[실측] 가린 창: hidden true, 4라운드(첫 ~480ms 후 998/1001/1001ms), 5.5초에 throttled true·gained 0 반환. 꺼낸 창: throttled false, 47라운드/12.1초/평균 257ms/+54건.** ⚠️ **반환까지의 시간(5.5초)은 상수가 아니다 — 3.1~5.5초로 변한다.** 첫 라운드가 콜드 스타트로 600ms 이하로 나오면 3연속을 채우는 데 4라운드가 걸려 ~5.5초, 첫 라운드부터 600ms를 넘으면 3라운드로 끝나 ~3.1초다(2026-09-09 검증 회차 실측: 3라운드 1088/1000/999ms, 3.088초). **판정 기준은 `3라운드 연속 600ms 초과`이지 경과 시간이 아니다** — 다음 회차는 시간이 5.5초와 다르다는 이유로 결함을 올리지 말 것. (같은 표현이 `SKILL.md` 636행 기준 334행 주석 `실측: 5.5초에 반환`에도 있다. 이번 정정 회차는 SKILL.md를 건드리지 않기로 해 그대로 두었다 — 다음 수정 회차에 함께 고칠 후보다.) |
| 결함 2 | **Step 4-1 `_parse`의 별점 읽기 주석**(599행 기준 221행 → **636행 기준 231~235행**. ⚠️ 636행 파일의 221행은 카드 경계 주석 = 결함 4 대상이므로 옛 번호를 그대로 쓰지 말 것) 교체: 읽기 순서 명시, "aria-label 실측 100%" 삭제 → "2026-09-09 실측 aria-label 0개, 381/381 SVG 색상 경로가 실제 경로". 신뢰도 판단 한 줄: 381건 중 이상값·파싱 실패 0, 분포 0/0/0/7/374, 카드 안 svg 구성(별 5개 16px + 아이콘 12px 검정)이라 오탐 여지 없음, `c <= 5` 가드로 44 방지 → 현재로서는 신뢰. aria-label 경로 유지. **트러블슈팅 `별점이 44 같은 이상값` 행**(599행 기준 586행 → **636행 기준 620행**)도 교체. 점검표 "되돌리면 안 되는 것" 행 교체 | 진단 회차 실측 |
| 결함 3 | **`## Step 0: 환경 확인`의 bash 블록**(599행 기준 49행) 한 줄 → 세 줄(`FOLDER_OK/NO_FOLDER`, `OPENPYXL_OK/NO_OPENPYXL`, `LOCKED/UNLOCKED`) + 각 실패의 판정 추가. **636행 기준 코드 49~51행, 판정 54~57행** | **[실측] PC에서: 정상 경로 FOLDER_OK/OPENPYXL_OK/UNLOCKED, 없는 폴더·없는 모듈 → NO_FOLDER/NO_OPENPYXL, 빈 잠금파일 생성 시 LOCKED(테스트 파일은 삭제 권한 받아 제거, 원본 엑셀 6,801 bytes 그대로)** |
| 결함 4 | 네 곳. 괄호 안은 (599행 기준 → **636행 기준**). **`## Step 4` 도입문**(199 → **207**) "약 6개" → "최상단 6개, 스크롤 중 최대 15개(2026-09-09)" / **`## 설계 근거` 2번째 불릿과 `### 4-2` 첫 불릿**(26·340 → **26·365**) "205건 약 50초" → "190건 약 50초(2회)+종료확인 1회, 96건 약 23초(2026-09-09)" / **`_parse` 카드 경계 주석**(213 → **221**) "182/182" → "381/381(2026-09-09)" / **`### 4-3` 저점수 희소성 문단**(383 → **417**) 6개월 저점수 수치에 "2026-09-09 재확인" | 진단 회차 실측 |
| 결함 5 (배너) | **결함 아님 — 코드 변경 없음.** 실물: `div.TemporaryDeliveryRegionAutoApplyFloatingBanner-module__rFic`, `position: fixed`, `z-index: 101`, role·aria-modal 없음, 뷰포트 하단 중앙(x 597~1307, y 715~839 @ innerHeight 855), 버튼 3개(`자세히보기`, `설정하기`, `aria-label="닫기"` svg). 기간 버튼(y 532~596)·기간 다이얼로그(y 156~699)·`적용`(y 631~679)·`최근 30일` 라벨과 겹치지 않음. `elementFromPoint` 세 곳 모두 본래 요소 반환 — **단 이 수정 회차 측정 당시의 다이얼로그 개폐 상태를 기록하지 않았다(측정 조건 미기록).** 2026-09-09 검증 회차가 조건을 명시해 재측정한 결과: **기간 다이얼로그가 열린 상태**에서 `적용`(y 631~679)과 `최근 30일` 라벨(y 306~330)은 본래 요소를 반환하고, **기간 버튼**(y 532~596)만 본래 요소가 아닌데 이는 배너가 아니라 **열린 다이얼로그(y 156~699)가 그 좌표를 덮기 때문**이다(배너는 y 715부터라 무관). 즉 배너로 인한 가로채기는 어느 상태에서도 없다. 다음 회차는 **다이얼로그 열린 상태 / 닫힌 상태를 각각 적어** 재측정할 것. 스킬은 JS `.click()`을 쓰므로 겹쳐도 가로채지 못한다. 뷰포트 가림은 하단 124px뿐이며 DOM 파싱에 영향 없음 → 개선안 ④(c)로 이월 | 수정 회차 실측 |
| 개선안 ② | `_parse`: 게시중단 카드를 `window._blocked[no]`에 기록(재검사 방지 포함). 4-3 반환에 `blocked`·`countMatch`(ET null이면 null). `ok` 판정 불변. 최종 보고 표에 `게시중단` 열 추가, 트러블슈팅에 `countMatch: false` 행 추가. **636행 기준: 초기화 212, 재검사 방지 219, blocked 기록 227, 4-3 반환 399·402, 보고 표 597, 트러블슈팅 637** | 진단 회차 189+1=190; 수정 회차 참제육 107→161건 수집 중 blocked 0 |
| 점검표 | 개정안 10건 전부 + "클라우드 push 403 → PC에서 push" 절차, 2회 커밋 흐름, 기준선 형식에 "수정 기록" 절 추가. v2 | — |

수정 중 관찰(다음 회차 참고): (1) 계정B로 로그인된 상태에서 계정A 매장 URL(14698107)에 가면 `등록된 가게가 없` 문구가 뜬다 — Step 2의 `noShop` 판정이 실제로 그 경로를 잡음[실측]. (2) 숨김 상태에서는 전화면 `LoadingBackdrop-module`(fixed, z-index 301)이 사라지지 않고 남아 있었다(카드 7개·전체(940) 로드 후에도 3초 이상). JS `.click()`에는 영향 없으나 좌표 클릭 fallback에는 영향 가능[추론]. 창을 꺼낸 뒤에는 `elementFromPoint`가 본래 요소를 반환했다.

수정 회차 검사: 저장소 SKILL.md의 javascript 코드 블록 6개를 `node --check`(top-level await 래핑), bash 블록 3개를 `bash -n`으로 문법 검사 통과. 최종 636행.

## 정정 기록 (2026-09-09 정정 회차 — 기록 문구만)

검증 세션(수정과 다른 세션)이 수정 회차 결과를 실물에서 재검증한 결과 **코드·동작의
재현 실패는 0건**이었다. 결함 1~4·개선안 ①②가 저장소 `SKILL.md` 636행에 전부 반영돼
있고 실측 근거도 재현됐다(3매장 Step 0~4-3 실행, Step 5 미실행, 원본 엑셀 md5 불변).

대신 **기록 문구 4건**이 틀린 것으로 확인돼 이 회차에 고쳤다. **네 건 모두 "코드는
정상, 기록 문구만 틀림" 유형이다** — `SKILL.md`는 이번에 손대지 않았고, 고친 것은
`audit/` 두 파일뿐이다.

| # | 무엇이 틀렸나 | 어떻게 고쳤나 | 파일 |
|---|---|---|---|
| 1 | **머리말과 [마무리] 절이 서로 다른 지시.** 머리말 7행은 `audit/last-audit.md` 와 **"같은 커밋으로"** push 하라고 했고, [마무리] 절은 **"커밋은 두 번이다"** 라고 했다. 개정안 6이 [마무리]에만 적용되고 머리말은 옛 문구 그대로였다. 점검표는 위에서부터 읽으므로 **다음 회차가 틀린 지시를 먼저 본다** | 머리말을 [마무리] 절과 같은 2회 커밋 규칙으로 교체하고, "이 머리말과 그 절은 항상 같은 말을 해야 한다"를 명시. 두 파일 전체에서 `같은 커밋` 을 재검색해 남은 1건이 [마무리] 절의 **역사적 인용**(옛 문구를 왜 버렸는지 설명하는 문장)임을 확인하고 그대로 보존 | checklist.md |
| 2 | **"수정 기록" 절의 행 번호가 전부 599행(수정 전) 기준.** 수정 후 파일은 636행이다 | 아래 "행 번호 정정 대조표"대로 636행 기준으로 다시 세어 고치고, **절 이름·함수명을 함께 병기**. 옛 번호는 `(599행 기준 N)`으로 남겨 이력을 보존 | last-audit.md |
| 3 | **`throttled` 반환까지의 "5.5초"가 상수처럼 적힘.** 실제로는 3.1~5.5초로 변한다 | `3.1~5.5초(첫 라운드 콜드 스타트 유무에 따라 변동)`로 고치고, **판정 기준은 시간이 아니라 `3라운드 연속 600ms 초과`** 임을 개선안 ① 표와 수정 기록 양쪽에 명시 | last-audit.md |
| 4 | **결함 5의 `elementFromPoint` 측정 조건 미기재.** 기간 다이얼로그 개폐 상태에 따라 결과가 달라지는데 어느 상태였는지 적히지 않았다 | 수정 회차의 조건은 **기록되지 않았다는 사실 그대로** 남기고(지어내지 않음), 검증 회차가 조건을 명시해 재측정한 결과를 덧붙임 | last-audit.md |

### 2번이 위험했던 이유 — 조용히 틀리는 종류였다

행 번호가 밀리면 보통 없는 행이나 엉뚱한 빈 줄을 가리켜 금방 들킨다. 그런데 이 건은
**636행 파일의 221행에도 실재하는 주석이 있었다.**

- 기록: "결함 2 — **221행** 주석 교체(별점 읽기 경로)"
- 636행 파일의 221행: `// 카드 경계는 이 클래스가 정상·특수 리뷰 모두에서 단일 리뷰만 정확히 감싼다(실측 2026-09-09: 381/381, no_card·merged 경고 0).`
  → 이건 **결함 4(카드 경계 수치 갱신)** 의 대상이다.
- 별점 주석의 실제 위치는 **231~235행**이다.

즉 다음 회차가 636행 기준으로 "결함 2가 반영됐는지" 확인하러 221행을 열면 결함 4의
결과물을 보게 되고, **두 결함을 뒤바꿔 판정할 뻔했다.** 검증 회차가 두 파일을 나란히
대조하지 않았으면 넘어갔을 건이다.

이 때문에 checklist.md `[판정 기준]`에 개정안 1건을 신설했다(아래).

### 행 번호 정정 대조표 (수정 기록 절)

| 항목 | 599행 기준(옛 기록) | **636행 기준(정정)** | 가리키는 것 |
|---|---|---|---|
| 결함 1 (a) | 26 | **26** (우연히 동일) | `## 설계 근거` 2번째 불릿 |
| 결함 1 (c) | 592 | **626·627·628** | `## 트러블슈팅` 감속/정지/Step 2 hidden 3행 |
| 결함 2 (주석) | 221 | **231~235** | Step 4-1 `_parse` 별점 읽기 주석 |
| 결함 2 (표) | 586 | **620** | 트러블슈팅 `별점이 44 같은 이상값` 행 |
| 결함 3 | 49 | **49~51**(코드) + **54~57**(판정) | `## Step 0` bash 블록 |
| 결함 4 (DOM 카드) | 199 | **207** | `## Step 4` 도입문 |
| 결함 4 (소요 수치) | 26·340 | **26·365** | 설계 근거 불릿 · `### 4-2` 첫 불릿 |
| 결함 4 (182/182) | 213 | **221** | `_parse` 카드 경계 주석 |
| 결함 4 (6개월 저점수) | 383 | **417** | `### 4-3` 저점수 희소성 문단 |
| 개선안 ② | (없었음) | **212·219·227·399·402·597·637** | `_blocked` 초기화·재검사 방지·기록 / 4-3 반환 / 보고 표 / 트러블슈팅 |

전부 636행 파일에서 해당 줄을 직접 열어 확인했다. 추정으로 옮긴 것은 없다.

### 점검표 개정안 (이번 정정 회차 신설 — checklist.md v3에 반영함)

11. **수정 회차의 기록은 행 번호 대신 절 이름·함수명으로 가리킬 것.** 위 2번 사고의
    재발 방지. 결함을 **지적**할 때는 지금 보고 있는 파일의 행 번호 + 원문 인용을
    그대로 유지하되(점검표의 "인용할 수 없으면 지적하지 마라" 규칙은 불변), **수정
    기록**에는 절·함수로 적고 행 번호를 병기할 거면 몇 행짜리 파일 기준인지 붙인다.
    → `[판정 기준]` 절에 추가함. 넣은 이유: 이 문제는 이번이 처음이 아니고(다른 스킬
    점검에서도 반복), 무엇보다 **틀려도 조용해서** 사람이 잡아내기 어렵다. 반면 절
    이름은 수정으로 밀리지 않아 비용이 거의 없다.

### 이번 정정 회차가 하지 않은 것

- `SKILL.md` 는 **건드리지 않았다.** 코드·동작 재현 실패가 0건이었고 사용자가 손대지
  말라고 지시했다. 다만 `SKILL.md` 636행 기준 **334행** 주석에도 `실측: 5.5초에 반환`
  이라는 같은 표현이 남아 있다 — 3번과 같은 성격이므로 **다음 수정 회차에 함께 고칠
  후보**로 남긴다(이번엔 의도적으로 두었다).
- 재패키징·재업로드 하지 않았다. 설치본은 여전히 진단 시점 버전(`499afc07…`)이다.

## 교차 정정 기록 (2026-09-09 — 쿠팡 스킬 점검에서 나온 개정안 이관)

**이 회차는 점검이 아니다.** 같은 날 쿠팡 리뷰 스킬 1회차 점검에서 실측으로 드러난
개정안 가운데 **배민 쪽에도 그대로 해당하는 3건**을 옮겨 적용한 것이다. 배민 스킬을
새로 진단해서 나온 결함이 아니다.

두 스킬은 같은 점검표 계보에서 나왔다 — 배민 점검표 v1 이 같은 날 saero·hometax
점검표를 옮겨 만든 것이고, 쿠팡 점검표도 같은 뿌리다. 그래서 **한쪽에서 발견된 결함이
다른 쪽에도 거의 그대로 남아 있다.** 실제로 이번 3건 중 2건(백업 경로)이 그랬고,
나머지 1건(설치본 경로)은 배민 점검표에서는 이미 고쳐졌으나 README 에만 남아 있었다.
**다음 회차는 "쿠팡 개정안 중 배민에 해당하는 것이 있는지"를 대조 항목으로 삼아라.**

| # | 파일·위치 | 고친 것 | 근거 |
|---|---|---|---|
| 1 | `audit/checklist.md` [시작 전 확인 ③] 백업 명령 | `$HOME/skillwork` → `$HOME/mnt/claude/backup/` (`mkdir -p` 유지). "세션별 홈이라 세션 종료 시 접근 불가" 사유를 본문에 명시 | 2026-09-09 쿠팡 점검 실측: `$HOME/skillwork` 는 세션별 홈이라 세션이 끝나면 접근할 수 없다. 거기 둔 백업은 백업 구실을 못 한다 |
| 2 | `SKILL.md` Step 5 저장 실패 대체 경로 | `~/skillwork/배민_저점수리뷰_backup.xlsx` → `~/mnt/claude/backup/배민_저점수리뷰_backup.xlsx`, 앞에 `os.makedirs(..., exist_ok=True)` 추가 | 같음. 엑셀 저장이 실패했을 때 떨어뜨리는 자리인데 세션과 함께 사라지면 **정작 필요할 때 못 꺼낸다.** 쿠팡 스킬이 같은 회차에 같은 자리를 같은 규칙으로 고쳤다 |
| 3 | `README.md` 8행 | `/mnt/skills/plugins/baemin-review/SKILL.md` → "스킬 호출 헤더의 `Base directory for this skill:` 줄이 알려준다" (2026-09-09 실측 경로 병기) | 그 경로는 이 환경에 없다. `checklist.md` 41행은 이미 이 표현으로 고쳐져 있었고 README 만 옛 문구로 남아 있었다 |

**의도적으로 두 것 — `skillwork` 가 남아 있는 자리는 전부 "임시 작업 공간" 용도다.**

- `SKILL.md` 439·444·457행 — 중간 산출물 `baemin.json` 의 작업 공간. 444행이
  "사용자에게 보이지 않는 작업 공간이다. 사용자 폴더에 임시 파일을 만들지 않는다"는
  이유를 이미 적고 있다. 세션 안에서만 쓰이므로 세션과 함께 사라져도 문제가 없다.
- `audit/checklist.md` 326행 — PC push 절차의 작업용 clone 경로. 임시 작업 공간이 맞다.
- `audit/last-audit.md` 8·80행 — 1회차 기록의 원문. 지난 기록은 고치지 않는다.

**바꾼 것은 백업 성격의 경로 두 개뿐이다.** 임시 작업 공간과 백업은 성격이 다르다 —
세션과 함께 사라져도 되는 것과, 사라지면 안 되는 것.

**이번 회차가 하지 않은 것**

- `SKILL.md` 의 판정 로직은 건드리지 않았다. diff 는 경로 문자열 1줄 + `os.makedirs` 1줄뿐.
- 재패키징·재업로드 하지 않았다(사용자 지시). `SKILL.md` 가 바뀌었으므로 **재패키징이
  필요하다** — 사용자가 따로 지시할 때까지 설치본은 이전 버전이다.
- **커밋 서명은 손대지 않는다(사용자 결정, 2026-09-09).** GitHub 의 `Unverified` 는 GPG
  서명이 없다는 뜻일 뿐이고, 이 저장소의 커밋은 실제로 사용자 PC·사용자 계정에서 올라간
  것이 맞다. 기록이 사실 그대로인 편이 낫다는 판단이므로 author 를 바꾸거나 force push
  하지 마라. 점검표 [마무리]의 PC push 절차가 사용자 이메일·`LeeKwanBeom` 을 쓰는 것도
  같은 이유다. **다음 회차는 이 항목을 다시 묻지 마라.**

## 다음 점검에서 대조할 것

- **쿠팡 리뷰 스킬의 그 회차 개정안 중 배민에도 해당하는 것이 있는지.** 두 스킬은 같은 점검표 계보라 한쪽 결함이 다른 쪽에도 남아 있다(2026-09-09 교차 정정에서 3건 확인). 반대 방향(배민 → 쿠팡)도 같이 본다.
- 설치본 md5가 저장소 `SKILL.md`(수정 회차 버전)와 같은지 — 다르면 "재업로드가 안 된 것". 사용자가 다른 세션 검증 후 재패키징·재업로드하기로 함.
- 검증 세션이 볼 것: Step 2 hidden 게이트 문구가 실제로 사용자 응답을 기다리는지, `_scroll`의 `throttled` 조기 반환이 가린 창에서 재현되는지(3라운드 연속 >600ms), 꺼낸 뒤 같은 호출로 이어지는지, Step 0 세 줄 출력, 보고 표 `게시중단` 열, `countMatch`.
- `collected + blocked === expectedTotal`이 3매장에서 성립하는지. 반복 성립하면 `ok` 판정 편입 여부 결정.
- 45초 CDP 타임아웃 재현 여부와 그때의 탭 숨김 경과 시간(점검표 F 실험).
- 곱도리 닉네임 실패 1건(2026082400693614)의 카드 첫 줄 구조.
- 개선안 ③④⑤ 이월분. ④(c) 배너는 뷰포트 높이가 730px 아래인 환경에서 `적용` 버튼과 겹치는지.
- 실측 못 한 것: 로그아웃 상태 접근 시 URL 패턴, `expectedTotal === null` 강제 시 흐름.

# 수정 회차에 적용할 것 (점검표에서 옮김 — 다음 단계용)

- **수정은 저장소 `SKILL.md`에서 한다.** 설치본을 직접 고치면 세션이 끝나며 사라진다. 저장소에 push한 뒤 그것으로 패키징해라.
- **한 번의 수정 → 한 번의 패키징 → 한 번의 재설치.** 중간에 다른 세션을 열면 그 세션은 옛 캐시를 읽고 그 위에 수정한다. 먼저 한 수정이 조용히 사라진다.
- **수정 후 반드시 파일을 다시 열어 눈으로 확인하고 나서 "완료"라고 말해라.**
- **고친 뒤 실제 사이트에서 1회 실행해라.** 문서만 고치고 끝내지 마라.
- **같은 개념을 두 곳에서 고칠 때는 기준을 대조해라.** 26·121·346·592행(hidden), 221·586행(별점)은 같은 규칙이 여러 곳에 있다.
- **판정 조건을 완화하는 수정을 했으면 원래 잡히던 실패가 여전히 잡히는지 다시 확인해라.** `ok` 판정과 `expectedTotal` 관련이 특히 그렇다.
- **"되돌리면 안 되는 것" 표에 있는 것을 건드렸으면 그 표도 같이 갱신해라.**
- **수정과 검증은 다른 세션에서 한다.**
- 작업 경로 세 단계: 저장소 `SKILL.md` 수정 → push / `.skill` 재패키징 / **사용자가 Claude 설정에서 재업로드**. 재업로드 전에는 실행에 반영되지 않는다.

## 이전 기록

(없음 — 이번이 첫 기준선)

- 2026-09-09 · 카카오톡 전송(Step 6) 미복원 결정. 신규 저점수 리뷰 알림은 채팅창 보고표로 갈음한다. 6/19 세대 SKILL.md는 archive/SKILL_20260619.md 에 보존.

