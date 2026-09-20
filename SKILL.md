---
name: baemin-review
description: "배달의민족(배민) 셀프서비스에서 저점수(1~3점) 리뷰를 자동으로 수집해 엑셀에 저장하는 스킬. 사용자가 \"배민 리뷰 조회\", \"배민 리뷰 확인\", \"저점수 리뷰 체크\", \"배민 리뷰 해줘\", \"배민 스킬\" 등 배민 리뷰와 관련된 요청을 할 때 반드시 이 스킬을 사용할 것. 크롬으로 배민 셀프서비스에 접속해 최근 30일 리뷰를 DOM에서 수집하고, 저점수만 골라 \"전체\" 단일 시트 엑셀에 저장한다."
---

# 배민 저점수 리뷰 수집

## 매장 정보

계정 전환을 최소화하기 위해 같은 계정 매장을 먼저 처리한다.

| 순서 | 매장명 | shopId | 계정 |
|---|---|---|---|
| 1 | 김치찜의 정석 | 14698107 | A |
| 2 | 퍽퍽살이 싫어 내가 만든 곱도리 | 14853640 | A |
| 3 | 참 제육 | 14697934 | B |

URL: `https://self.baemin.com/shops/<shopId>/reviews`

계정A 두 매장을 마친 뒤, 참 제육 접속 **전에** 사용자에게 알린다:
> 계정A(김치찜·곱도리) 완료. 이제 참 제육(계정B) 차례입니다. 크롬에서 계정B로 로그인해 주시고 준비되면 알려주세요.

## 설계 근거 (건드리기 전에 읽을 것)

- **API를 쓰지 않는 이유**: `self-api.baemin.com`은 페이지와 다른 오리진이라, 브라우저 확장이 주입한 스크립트에서는 인증이 필요 없는 `/v2/maintenance`조차 전부 `Failed to fetch`로 차단된다(2026-09-05 실측, XHR·iframe 네이티브 fetch 모두 동일). 서버 503이나 인증 만료가 아니라 크로스 오리진 차단이므로 헤더를 맞춰도 뚫리지 않는다. DOM 수집이 유일한 경로다. (쿠팡이츠는 API가 same-origin이라 되는 것이고, 같은 방법을 배민에 적용할 수 없다.)
- **탭이 `document.hidden === true`로 시작할 수 있다 — 스크린샷·wait로는 못 푼다**: `tabs_context_mcp(createIfEmpty=true)`로 만든 새 창은 다른 창(보통 Claude 앱)에 가려져 `hidden: true`로 시작하는 것이 기본값에 가깝다(2026-09-09·09-20 실측, 새 탭 3회 모두). 사용자가 Claude 앱에서 답장을 쓰는 동안에도 크롬이 가려져 다시 `true`가 된다(2026-09-20 곱도리 실측). 그 상태에서는 두 가지가 동시에 일어난다. (1) **감속** — 백그라운드 탭 타이머 클램프로 `setTimeout(250)`이 약 1000ms에 깨어난다(실측 741/994/1007/994/1003ms, 2026-09-20 1257/995/1007/996/995ms). (2) **정지** — `requestAnimationFrame`이 2초 안에 한 번도 오지 않아 무한스크롤 로더가 돌지 않고, 리스트가 더 로드되지 않는다(실측: 12라운드·20초·gained 0, scrollHeight 4362 고정). 이 상태를 모르고 진행하면 190건 중 6건에서 멈춘다. `computer(screenshot)`·`computer(wait)`는 둘 다 `hidden`을 `false`로 바꾸지 못하고 프레임 1장만 강제한다(실측: 직후에도 hidden true, 타이머 ~1000ms). **유일한 해제 방법은 사용자가 크롬 창을 화면 앞으로 꺼내는 것**이며, 그 직후 타이머는 250ms대로 돌아온다(실측 251~261ms). 그래서 **Step 3(스크롤 직전)이 `hidden`을 게이트로 쓰고**, `_scroll`이 감속을 스스로 감지해 조기 반환한다. 팝업 닫기·로그인 확인·기간 필터 적용은 숨김 상태에서도 정상 동작하므로(2026-09-09·09-20 실측) 게이트 앞(Step 2)에 둔다. 스크린샷 깨우기·browser_batch 사이클은 쓰지 않는다.
- **숨김이 1분을 넘으면 연쇄 타이머가 분당 1회로 떨어질 수 있다**: 콜백 안에서 재예약하는 연쇄 `setTimeout`이 숨김 약 70초 뒤부터 60,000ms 간격으로 정렬되는 것을 실측했다(2026-09-20, 7회 연속 59,991~60,005ms). `_scroll`은 호출마다 새 task로 시작하고 3라운드 연속 600ms 초과면 반환하므로 그 전에 끝나지만, hidden 게이트와 `throttled` 조기 반환을 빼면 안 되는 이유가 하나 더다.
- **가시 상태의 속도**: 라운드 대기는 고정 250ms가 아니라 **적응형**(DOM 지문이 바뀌면 50ms 뒤 진행, 상한 250ms)이고 스크롤 스텝은 **1800px**(카드 약 2개, 카드 높이 중앙값 약 900px·DOM 윈도우 12~13카드 ≈ 10,000px)다. 2026-09-20 실측(수정 회차): 김치찜 175건(게시중단 1) 83라운드·6.9초, 곱도리 87건 40라운드·3.9초, 참 제육 91건 43라운드·4.2초, 라운드당 83~98ms — 같은 세션에서 돌린 고정 250ms·900px 코드(각 50초·20초·21초)와 리뷰번호 집합·별점·날짜·메뉴가 전건 동일했다. `_parse` 자체는 0.2~2ms라 라운드의 99%가 대기였다.
- **호출 수를 줄인 이유**: 2026-09-20 계측에서 매장당 도구 왕복·코드 생성이 72~134초로 사이트 렌더링(23~104초)보다 컸다. 큰 정의 블록(약 8KB)을 매장마다 다시 내보내는 것이 주범이라, 팝업·로그인·기간 필터·정의를 **Step 2 한 호출**로 합치고, 정의는 매장 1에서 같은 오리진 `localStorage`에 저장해 매장 2·3은 짧은 재주입 블록으로 되살린다(`eval` 은 이 사이트에서 허용됨 — 2026-09-20 실측). 재주입 키 `SID`는 세션마다 새로 정하며 **다른 세션의 정의를 재사용하지 않는다**(오래된 코드가 조용히 살아나는 것을 막는다).
- **`javascript_tool` 호출은 45초에서 CDP 타임아웃**이 난다. 배치 예산은 25초를 넘기지 않는다. 반환값은 약 1,000자에서 끝에 `[TRUNCATED]` 표식과 함께 잘리고, 쿼리스트링이 든 URL을 담으면 결과 전체가 `[BLOCKED: Cookie/query string data]`로 바뀐다 — 반환 JSON에 `location.href`를 넣지 않는다.
- **엑셀은 사용자 PC에서 직접 처리**한다(`device_bash`). 클라우드 컨테이너엔 `/sessions` 경로 자체가 없으므로 `find /sessions/*/mnt/...` 같은 탐색을 되살리지 말 것. 스테이징·전송·커밋 왕복도 필요 없다.

매장 처리가 실패해도 다음 매장은 계속 진행한다. 전체를 중단하는 조건은 Step 0(환경 확인 실패)뿐이다.

### 조용한 0건을 막는 세 가지 원칙 (2026-09-06 도입 — 되돌리지 말 것)

이 스킬의 가장 위험한 실패는 "조용히 0건"이다. DOM 스크롤이 중간에 끊겼는지, 리뷰가 정말 없는지 구분하지 못하면 **"저점수 리뷰 없음"이라는 잘못된 안심**을 주고, 이어지는 저장 단계가 기존 엑셀 행까지 지운다. 아래 셋은 그 사고를 막는 장치다.

1. **`expectedTotal`은 `0`과 `null`이 다르다.** `0`은 "그 기간에 리뷰가 없음이 확인됨"이고, `null`은 "총건수를 못 읽음 = 확인 필요"다. `null`에 `|| 0`을 붙이지 말 것. `null`인 매장은 스크롤은 끝까지 하되 **저장 단계에서 읽지도 쓰지도 않는다**(기존 행 보존).
2. **별점을 못 읽은 리뷰를 조용히 버리지 않는다.** `parseFail`로 세고, `[별점확인필요]`를 붙여 저점수 목록에 남기며, 최종 보고에 건수를 올린다. 4~5점으로 **확인된** 리뷰만 제외한다.
3. **매장 성공/실패는 `ok` 플래그로 판정한다.** `ok: true`는 "기간 필터가 적용됐고(다이얼로그 닫힘 + 기간 버튼의 날짜가 오늘−30일 ~ 오늘, ±1일) · 총건수를 읽었고 · **수집 + 게시중단이 전체(N) 이상**이고 · 별점 파싱 실패가 10% 이하이고 · 수집 도중 세션이 끊기지 않았다"를 전부 만족할 때만 붙는다. 기본값은 실패다. 저장 스크립트는 `ok !== true`인 매장을 통째로 건너뛴다.

> 실패한 매장도 **JSON 배열에서 빼지 않는다.** `ok: false`인 채로 그대로 넣는다. 저장 스크립트가 `ok`를 보고 스스로 건너뛴다 — 사람이 골라내지 않는다.

---

## Step 0: 환경 확인 + 실행 전 백업

`device_bash`로 폴더·파이썬·잠금파일을 확인한다. 세 검사는 **각각 따로** 출력한다 — 예전의 `A && B && C && echo LOCKED || echo UNLOCKED` 한 줄은 폴더나 openpyxl이 없어도 마지막 `|| echo UNLOCKED`가 찍혀 환경 실패가 통과로 보였다(2026-09-09 재현). 되돌리지 말 것.

```bash
[ -d "$HOME/mnt/claude" ] && echo FOLDER_OK || echo NO_FOLDER
python3 -c "import openpyxl" 2>/dev/null && echo OPENPYXL_OK || echo NO_OPENPYXL
[ -e "$HOME/mnt/claude/~\$배민_저점수리뷰.xlsx" ] && echo LOCKED || echo UNLOCKED
```

- 세 줄이 `FOLDER_OK` · `OPENPYXL_OK` · `UNLOCKED` 이어야 정상이다. 하나라도 빠지면 아래를 따른다.
- `NO_FOLDER` → 폴더가 연결되지 않은 것. 사용자에게 `claude` 폴더 연결을 요청한다. 수집은 진행하되 Step 5는 실행하지 않는다.
- `NO_OPENPYXL` → 사용자 PC에 openpyxl이 없다. `pip install openpyxl`을 시도하고 실패하면 Step 5를 건너뛴다.
- `LOCKED` → 사용자에게 엑셀을 닫아달라고 요청하고 재확인한다.
- `device_bash` 자체가 실패하면 → 사용자에게 "PC에 연결되어 있지 않아 엑셀 저장을 할 수 없다"고 알리고, 수집은 진행하되 결과를 채팅에 표로만 출력한다.

엑셀 경로는 `$HOME/mnt/claude/배민_저점수리뷰.xlsx` 이다.

`UNLOCKED`면 **실행 전 백업**을 사용자 폴더 안에 남긴다. 이 스킬은 스냅샷 동기화라 Step 5가 기존 행을 지우므로 백업이 유일한 되돌리기 수단이다. `$HOME/skillwork`나 `$HOME` 아래는 세션별 홈이라 세션이 끝나면 접근할 수 없으니(2026-09-09 실측) 거기에 두지 말 것.

```bash
mkdir -p "$HOME/mnt/claude/backup" && if [ -f "$HOME/mnt/claude/배민_저점수리뷰.xlsx" ]; then cp "$HOME/mnt/claude/배민_저점수리뷰.xlsx" "$HOME/mnt/claude/backup/배민_저점수리뷰_$(date +%Y%m%d_%H%M%S).xlsx" && echo "백업 완료: $(ls -t "$HOME/mnt/claude/backup/" | head -1)" || echo "백업 실패 — 중단하고 사용자에게 알린다"; else echo "백업 대상 없음(첫 실행)"; fi
```

백업은 `backup/` 폴더에 회차마다 쌓인다. 정리는 사용자 몫이며 스킬이 지우지 않는다. `백업 실패`가 찍히면 Step 5로 가지 않는다.

---

## Step 1: 탭 열기 (최초 1회)

```
tabs_context_mcp(createIfEmpty=true)
navigate(url=https://self.baemin.com/shops/14698107/reviews, tabId=<탭ID>)
```

여기서 이 세션의 정의 재주입 키 **`SID`**를 하나 정한다(예: 탭을 만든 시각 `20260920T2110`). 세 매장의 Step 2에 같은 값을 쓴다. **다른 세션에서 쓴 값을 다시 쓰지 않는다** — 매장 1의 전체판 블록이 항상 그 세션의 정의로 덮어쓴다.

매장이 바뀔 때는 같은 탭에서 `navigate`만 다시 한다. 페이지가 이동하면 `window`의 모든 상태가 초기화되므로 **매 매장마다 Step 2~4를 처음부터 다시 실행**한다(정의만 `localStorage`에서 되살린다).

---

## Step 2: 준비 — 팝업 닫기 + 로그인 확인 + 기간 필터 "최근 30일" + 파서·스크롤 정의 (매장마다, 1회 호출)

한 호출로 끝낸다. 매장 1(또는 재주입이 `NO_DEFS`인 매장)은 **2-A 전체판**, 매장 2·3은 **2-B 재주입판**을 쓴다. `<SHOP_ID>`, `<매장명>`, `<SID>`는 실제 값으로 치환한다. `find()`나 스크린샷을 쓰지 않는다.

### 2-A. 전체판 (정의 + 실행 + 정의 저장)

```javascript
// ===== 정의 =====
window._dismiss = function() {
  // 도킹된 챗봇 위젯이 role="dialog"로 잡혀 "닫을 팝업"으로 오인되는 사례가 있어 명시적으로 제외한다.
  const inView = el => { const r = el.getBoundingClientRect();
    return r.width > 0 && r.height > 0 && r.left < innerWidth && r.right > 0 && r.top < innerHeight && r.bottom > 0; };
  const isChatbot = el => !!(el.matches?.('[class*="ChatRoom-module"]') || el.querySelector?.('[class*="ChatRoom-module"]'));
  const isFilterDialog = el => /기간/.test(el.innerText || '') && /최근\s*\d+\s*(일|개월)/.test(el.innerText || '');
  for (const d of [...document.querySelectorAll('[role="dialog"],[aria-modal="true"]')].filter(e => inView(e) && !isChatbot(e))) {
    if (isFilterDialog(d)) continue;   // 우리가 여는 기간 다이얼로그는 절대 닫지 않는다
    const skip = [...d.querySelectorAll('button,a,[role="button"]')]
      .find(e => /보지\s*않기|다시\s*보지|hide|dismiss/i.test(e.innerText?.trim() || '') && inView(e));
    if (skip) { skip.click(); return 'dont_show'; }   // X 닫기는 지속성이 없어 재등장하므로 이쪽을 우선
    const x = d.querySelector('button[aria-label*="close" i],button[aria-label*="닫기"]')
      || [...d.querySelectorAll('button')].find(b => b.querySelector('svg') && !b.innerText?.trim());
    if (x) { x.click(); return 'x'; }
    document.dispatchEvent(new KeyboardEvent('keydown', { key: 'Escape', bubbles: true }));
    return 'esc';
  }
  return 'none';
};
window._dismissAll = async function(n = 3) {
  const out = [];
  for (let i = 0; i < n; i++) { const r = window._dismiss(); out.push(r); if (r === 'none') break; await new Promise(s => setTimeout(s, 400)); }
  return { out, gone: out[out.length - 1] === 'none' };
};

// 기간 필터. 적용 확인은 "다이얼로그 닫힘 + 기간 버튼 자신의 두 날짜가 오늘-days ~ 오늘(±1일)"이다.
// 본문 전체에서 라벨을 찾던 예전 방식(labelOk)은 열린 다이얼로그의 옵션 라벨을 잡아, 적용을 안 눌러도 true가 됐다(2026-09-20 재현). 되돌리지 말 것.
window._applyPeriod = async function(label = '최근 30일', days = 30, skipOpen = false, maxWait = 3000) {
  await window._dismissAll();   // 팝업이 열려 있으면 필터 버튼 클릭을 가로채서 엉뚱한 기간이 선택된다
  const inView = el => { const r = el.getBoundingClientRect(); return r.width > 0 && r.height > 0; };
  // 전체(N) — 쉼표 포함 숫자를 반드시 처리한다. "전체(1,460)"에서 \d+ 만 쓰면 1로 읽혀 수집이 1건에서 끝난다.
  const totalOf = () => { const m = document.body.innerText.match(/전체\s*\(([\d,]+)\)/); return m ? parseInt(m[1].replace(/,/g, ''), 10) : null; };
  const isOpen = () => [...document.querySelectorAll('[role="dialog"],[aria-modal="true"]')]
    .some(e => inView(e) && /기간/.test(e.innerText || '') && /최근\s*\d+\s*(일|개월)/.test(e.innerText || ''));
  const before = totalOf();
  let opened = 'skipped';
  if (!skipOpen) {
    opened = 'not_found';
    for (const p of ['최근 7일','최근 30일','최근 3개월','최근 6개월','날짜 직접 선택','기간']) {
      const el = [...document.querySelectorAll('button,[role="button"]')].find(e => e.innerText?.includes(p) && inView(e));
      if (el) { el.click(); opened = 'clicked:' + p; break; }
    }
    if (opened === 'not_found') {
      const el = [...document.querySelectorAll('button,[role="button"],div,span')]
        .find(e => /\d{4}\.\s*\d{1,2}\.\s*\d{1,2}.*~/.test(e.innerText?.trim() || '') && inView(e) && (e.innerText.trim().length || 0) < 60);
      if (el) { (el.closest('button,[role="button"]') || el).click(); opened = 'clicked_range'; }
      else return { ok: false, stage: 'button_not_found' };
    }
  }
  let waited = 0;
  while (!isOpen() && waited < maxWait) { await new Promise(s => setTimeout(s, 300)); waited += 300; await window._dismissAll(); }
  if (!isOpen()) return { ok: false, stage: 'dialog_not_opened', waited };

  // inView는 offsetParent를 쓰지 않는다 — position:fixed 요소에서 항상 null이라 다이얼로그 안 버튼을 못 찾는다
  const radio = [...document.querySelectorAll('input[type="radio"]')]
    .find(i => document.querySelector(`label[for="${i.id}"]`)?.innerText?.includes(label));
  if (!radio) return { ok: false, stage: 'radio_not_found', waited };
  radio.click(); radio.dispatchEvent(new Event('change', { bubbles: true }));

  await new Promise(s => setTimeout(s, 300));
  const apply = [...document.querySelectorAll('button')]
    .find(b => { const t = b.innerText?.trim() || ''; return (t === '적용' || (t.length <= 10 && /적용|확인|조회/.test(t))) && inView(b); });
  if (!apply) return { ok: false, stage: 'apply_not_found', waited };
  apply.click();

  // 전체(N) 갱신 폴링: 이전 값에서 바뀌고 non-null로 4연속(약 200ms) 같으면 진행, 상한 3초. 고정 2.5초 대기를 대체한다.
  // 2026-09-20 실측: 915 → null@76ms(과도기) → 90@244ms → 안정@469ms. 값이 안 바뀌는 경우(이미 30일 상태 등)만 3초를 다 쓴다.
  const tA = performance.now(); let v = totalOf(), last = null, stable = 0;
  while (performance.now() - tA < 3000) {
    await new Promise(s => setTimeout(s, 50));
    v = totalOf();
    if (v !== null && v !== before && v === last) { if (++stable >= 3) break; } else stable = 0;
    last = v;
  }
  const open = isOpen();
  const btn = [...document.querySelectorAll('button,[role="button"]')].find(e => /최근\s*\d+\s*(일|개월)/.test(e.innerText || '') && inView(e));
  const txt = btn?.innerText || '';   // 적용 후 버튼 텍스트: "최근 30일\n2026. 8. 21 (금) ~ 2026. 9. 20 (일)"
  const mm = txt.match(/(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})[^~]*~\s*(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})/);
  const day = (y, m, d) => Date.UTC(+y, +m - 1, +d) / 86400000;
  const now = new Date(); const t0 = day(now.getFullYear(), now.getMonth() + 1, now.getDate());
  const startOk = !!mm && Math.abs(day(mm[1], mm[2], mm[3]) - (t0 - days)) <= 1;   // ±1일: 자정 경계
  const endOk = !!mm && Math.abs(day(mm[4], mm[5], mm[6]) - t0) <= 1;
  return { ok: true, opened, waited, totalWaitMs: Math.round(performance.now() - tA),
    filterOk: !open && startOk && endOk, dialogOpen: open, labelOk: txt.includes(label), range: mm ? mm[0] : null,
    expectedTotal: v };
};

// ===== 파서 =====
window._all = {}; window._warn = []; window._blocked = {};
window._parse = function() {
  const DT = ['가게배달','한집배달','알뜰배달','배달','포장','직접배달','배민배달','가게포장','픽업'];
  const PK = ['사장님께만 보이는','파트너님에게만','파트너에게만','비공개 리뷰','점주에게만'];
  // 리뷰 전체가 비공개임을 알리는 라벨 한 줄(2026-09-20 실측 "파트너님에게만 보이는 리뷰입니다."). 본문이 아니라 플래그다 —
  // 이 줄 뒤 텍스트를 다시 잡으면 본문이 두 번 들어간다(2026-09-20 결함 3, 9건 중 7건).
  const PK_LABEL = /^(파트너님?|사장님|점주)(에게|께)만\s*보이는\s*리뷰입니다\.?$/;
  // 닉네임 후보에서 제외할 메타 줄 — 정확 패턴만. 부분 일치(/주문/ 등)는 "내가주문한" 같은 닉네임을 기각했다(2026-09-20 결함 4).
  const META = [/^\d+회\s*주문\s*고객$/, /^리뷰번호/, /^배달리뷰$/, /^사장님/, /^답글/, /^주문메뉴$/, /^\(최근/, /^\d{4}년\s*\d{1,2}월/, PK_LABEL];
  for (const s of document.querySelectorAll('span')) {
    const t = s.innerText?.trim(); if (!t) continue;
    const m = t.match(/^리뷰번호\s+(\d+)$/) || t.match(/^(20\d{14})$/);
    if (!m || window._all[m[1]] || window._blocked[m[1]]) continue;
    const no = m[1];
    // 카드 경계는 이 클래스가 정상·특수 리뷰 모두에서 단일 리뷰만 정확히 감싼다(실측 2026-09-09 381/381, 2026-09-20 349/349, no_card·merged 경고 0).
    // 부모로 walk-up하는 예전 방식은 이웃 리뷰까지 병합해 데이터를 섞어버렸다 — 되살리지 말 것.
    const card = s.closest('[class*="ReviewContent-module"]');
    if (!card) { window._warn.push('no_card:' + no); continue; }
    // 우리가 신고해 내린 리뷰 — 수집 제외. 단 건수는 센다: 페이지의 전체(N)에는 포함되므로
    // collected + blocked === expectedTotal 로 수집 완료를 검증할 수 있다(2026-09-09 189 + 1 = 190, 2026-09-20 173 + 1 = 174).
    if (card.innerText?.includes('게시중단 요청으로 인해')) { window._blocked[no] = 1; continue; }
    if ((card.innerText.match(/리뷰번호/g) || []).length > 1) { window._warn.push('merged:' + no); continue; }

    const text = card.innerText || '';
    // 별점 읽기 순서: aria-label → data-* → SVG 색상 → img alt.
    // 2026-09-09 실측: 카드 안에 aria-label 요소가 0개라 381/381건 전부 SVG 색상 경로로 읽혔다(2026-09-20 349/349 동일). 즉 **현재 사이트에서 실제 경로는 색상 fallback**이다.
    // aria-label 경로는 사이트가 되돌아올 때를 대비해 남긴다 — 지우지 말 것.
    // 색상 경로 신뢰도: 730건 중 범위 밖 값(44 같은 이상값)·파싱 실패 0건, 4점 카드 본문 대조 일치. 카드 안 svg는 별 5개(16px, #FFC600 또는 회색)와
    // 12px 아이콘 1개(검정)뿐이라 오탐 여지가 없었다. 44 이상값은 카드 병합 시절의 산물이며 `c <= 5` 가드가 막는다. 현재로서는 신뢰할 수 있다고 본다.
    let stars = -1;
    for (const svg of card.querySelectorAll('svg')) {
      const a = svg.getAttribute('aria-label') || svg.closest('[aria-label]')?.getAttribute('aria-label') || '';
      const mm = a.match(/(\d)점/); if (mm) { stars = +mm[1]; break; }
    }
    if (stars < 0) {
      const r = card.querySelector('[data-rating],[data-score],[data-star]');
      const v = r && parseInt(r.getAttribute('data-rating') || r.getAttribute('data-score') || r.getAttribute('data-star'));
      if (v >= 1 && v <= 5) stars = v;
    }
    if (stars < 0) {
      let c = 0;
      for (const svg of card.querySelectorAll('svg')) {
        for (const p of svg.querySelectorAll('path,polygon')) {
          const f = ((p.getAttribute('fill') || '') + (p.style?.fill || '') + (getComputedStyle(p).fill || '')).toUpperCase();
          const rgb = f.match(/RGBA?\((\d+),\s*(\d+),\s*(\d+)/);
          if (/FFC600|FFB800|FFCC00|FFCA28|FFD600|FFB300/.test(f) || (rgb && +rgb[1] > 180 && +rgb[2] > 130 && +rgb[3] < 80)) { c++; break; }
        }
      }
      if (c > 0 && c <= 5) stars = c;   // 범위 밖 값은 버린다(병합 카드 오탐 방지)
    }
    if (stars < 0) {   // 별점 UI가 <img>로 바뀌는 경우 대비
      for (const img of card.querySelectorAll('img[alt]')) {
        const mm = (img.getAttribute('alt') || '').match(/(\d)\s*점/); if (mm) { stars = +mm[1]; break; }
      }
    }
    if (stars < 0) window._warn.push('star_fail:' + no);

    const dm = text.match(/(\d{4})년\s*(\d{1,2})월\s*(\d{1,2})일/);
    const date = dm ? `${dm[1]}-${String(dm[2]).padStart(2,'0')}-${String(dm[3]).padStart(2,'0')}` : '';

    const lines = text.split('\n').map(x => x.trim()).filter(Boolean);
    const okNick = n => n && n.length < 30 && !n.startsWith('(') && !META.some(re => re.test(n)) && !DT.includes(n);
    let nick = '';
    const di = lines.findIndex(l => DT.includes(l));
    if (di >= 0 && okNick(lines[di + 1])) nick = lines[di + 1];
    if (!nick) { const oi = lines.findIndex(l => /^\d+회\s*주문\s*고객$/.test(l)); if (oi > 0 && okNick(lines[oi - 1])) nick = lines[oi - 1]; }
    if (!nick && date) { const li = lines.findIndex(l => l.includes(dm[1])); if (li > 0 && okNick(lines[li - 1])) nick = lines[li - 1]; }

    const menu = (text.match(/주문메뉴\s*\n?\s*([^\n]+)/) || [])[1]?.trim() || '';

    const ms = text.indexOf('주문메뉴'), di2 = dm ? text.indexOf(dm[0]) : -1;
    const partnerOnly = lines.some(l => PK_LABEL.test(l));
    let pub = '';
    if (ms > 0 && di2 >= 0) {
      pub = text.substring(di2, ms).split('\n').map(x => x.trim())
        .filter(l => l && !/^\d{4}년/.test(l) && !l.includes('리뷰번호') && !l.includes('주문 고객')
          && !l.includes('누적 주문') && !DT.some(d => l.includes(d)) && !PK.some(k => l.includes(k)))
        .join(' ').trim();
    }
    if (!pub && partnerOnly) {   // 라벨은 있는데 주문메뉴가 없는 카드(2026-09-20 실측 2건): 라벨 다음 줄부터 배달리뷰·사장님·주문메뉴 전까지가 본문
      const li = lines.findIndex(l => PK_LABEL.test(l)); const a = [];
      for (let i = li + 1; i < lines.length; i++) { if (/^(주문메뉴|배달리뷰|사장님)/.test(lines[i]) || /^\d{4}년/.test(lines[i])) break; a.push(lines[i]); }
      pub = a.join(' ').trim();
    }
    let partner = '';
    if (!partnerOnly) {   // 키워드 뒤에만 텍스트가 오는 "별도 비공개 구간" 구조가 다시 나타날 때 대비. 라벨 구조에서는 본문을 두 번 잡으므로 쓰지 않는다.
      for (const kw of PK) {
        const ki = text.indexOf(kw); if (ki < 0) continue;
        const after = text.substring(ki + kw.length);
        let stop = after.length;
        for (const sp of ['주문메뉴','배달리뷰','답글']) { const si = after.indexOf(sp); if (si >= 0 && si < stop) stop = si; }
        const pl = after.substring(0, stop).split('\n').map(x => x.trim()).filter(l => l && l.length > 1);
        if (pl.length) { partner = pl.slice(0, 5).join(' ').trim(); break; }
      }
    }
    // 배달리뷰 칩(좋아요/아쉬워요 + 세부 태그). 본문이 없는 카드(2026-09-20 실측 349건 중 105건)에서만 리뷰내용으로 쓴다 — 본문이 있으면 본문만.
    let chip = '';
    { const dr = lines.indexOf('배달리뷰');
      if (dr >= 0) { const a = []; for (let i = dr + 1; i < lines.length; i++) { if (/^(사장님|삭제|수정)/.test(lines[i]) || /^\d{4}년/.test(lines[i])) break; a.push(lines[i]); } chip = a.join(', '); } }
    let review = '(텍스트 없음)';
    if (pub && partner) review = `${pub} / [파트너전용] ${partner}`;
    else if (pub) review = (partnerOnly ? '[파트너전용] ' : '') + pub;
    else if (partner) review = `[파트너전용] ${partner}`;
    else if (chip) review = `[배달리뷰] ${chip}`;

    if (!nick) window._warn.push('nick_fail:' + no);
    if (!menu && ms > 0) window._warn.push('menu_fail:' + no);
    window._all[no] = { reviewNo: no, stars, date, nickname: nick, menu, reviewText: review };
  }
  return Object.keys(window._all).length;
};

// ===== 스크롤 =====
// 배치 예산 25초 (CDP 타임아웃 45초보다 충분히 아래).
// 수집 도중 세션이 끊기면 DOM이 비어 gained===0 → "정상 종료"처럼 보인다.
// 매 라운드 로그인 상태를 확인해 치명 실패로 올린다. (쿠팡의 401 fatal에 대응하는 장치)
window._lost = () => /login|signin|auth/i.test(location.href)
  || document.body.innerText.includes('등록된 가게가 없');
window._atBottom = () => Math.round(scrollY) + innerHeight >= document.body.scrollHeight - 2;
// DOM 지문: 리뷰번호 span 수 | 마지막 리뷰번호 | scrollHeight. 카드 "수"는 윈도잉으로 12~13 고정이라 신호가 아니다(2026-09-20 실측 55라운드 중 11회만 변화).
window._fp = () => { let last = '', n = 0; for (const s of document.querySelectorAll('span')) { const t = s.textContent; if (t && t.length < 30 && /^리뷰번호\s+\d+$/.test(t.trim())) { n++; last = t; } } return n + '|' + last + '|' + document.body.scrollHeight; };
// 적응형 대기: 지문이 바뀌면 settleMs 뒤 진행, 상한 capMs. 폴링은 지문만 보고 _parse는 라운드당 1회 — 예전에 폴링마다 파서를 부르던 구조가 느렸다(되돌리지 말 것).
// 2026-09-20 실측: scrollBy 뒤 scrollHeight 변화 25~65ms(중앙값 42), 라운드 85~100ms. 숨김 상태에서는 25ms 폴도 1초로 클램프돼 라운드 ≈1000ms가 되고 아래 throttled 검사가 그대로 잡는다.
window._wait = async function(capMs = 250, settleMs = 50, pollMs = 25) {
  const t0 = performance.now(); const f0 = window._fp();
  while (performance.now() - t0 < capMs) {
    await new Promise(r => setTimeout(r, pollMs));
    if (window._fp() !== f0) { const rem = Math.min(settleMs, capMs - (performance.now() - t0)); if (rem > 0) await new Promise(r => setTimeout(r, rem)); return Math.round(performance.now() - t0); }
  }
  return Math.round(performance.now() - t0);
};
window._scroll = async function(budgetMs = 25000, target = null, step = 1800) {
  const t0 = performance.now(); let rounds = 0, stuck = 0, authExpired = false, throttled = false, wiggles = 0, bottom = false;
  const before = Object.keys(window._all).length;
  const roundMs = [], waitMs = [];
  while (performance.now() - t0 < budgetMs) {
    if (window._lost()) { authExpired = true; break; }   // 즉시 중단. 재시도해도 소용없다
    rounds++;
    const rt = performance.now();
    const b = Object.keys(window._all).length; const shB = document.body.scrollHeight;
    window.scrollBy(0, step);   // 1800px ≈ 카드 2개. 2026-09-20 실측: 1800·2700·4500 모두 900과 집합 동일, DOM 윈도우(≈10,000px)의 1/5인 1800을 택함
    waitMs.push(await window._wait(250, 50, 25));
    const a = window._parse();
    roundMs.push(Math.round(performance.now() - rt));
    // 감속 자가 감지. 정상 라운드 ≈ 85~100ms(2026-09-20 실측, 적응형 대기). 크롬 창이 가려지면 백그라운드 타이머 클램프로 라운드가 ~1000ms가 되고 렌더링이 멈춰 gained도 0이 된다.
    // 임계 600ms = 정상 최대의 수 배이면서 클램프 값(1000)의 60%. 3라운드 연속을 요구하는 이유: 첫 라운드는 콜드 스타트로 480~741ms까지 관측됐고, 단발 지연을 감속으로 오판하면 안 된다.
    // 감속이면 예산을 다 쓰지 않고 즉시 반환한다(실측: 3.1~5.5초에 반환 — 첫 라운드 콜드 스타트 유무로 변한다. 판정 기준은 시간이 아니라 3라운드 연속 600ms 초과). 호출부가 사용자에게 창을 꺼내달라고 요청한다.
    if (roundMs.length >= 3 && roundMs.slice(-3).every(ms => ms > 600)) { throttled = true; break; }
    // 완전 = 수집 + 게시중단 = 전체(N). collected만 보면 게시중단이 있는 매장은 target에 영원히 못 닿아 예산을 다 쓴다(2026-09-20 결함 5: 김치찜 무진전 2회·50초).
    if (target && a + Object.keys(window._blocked).length >= target) break;
    if (a === b) {
      if (++stuck >= 3) {   // wiggle: 위로 살짝 올렸다 크게 내려 lazy-load 재유도
        wiggles++;
        await window._dismissAll();
        window.scrollBy(0, -600); await new Promise(r => setTimeout(r, 200));
        window.scrollBy(0, 1500); await new Promise(r => setTimeout(r, 600));
        const a2 = window._parse(); stuck = 0;
        // 바닥 도달: wiggle 뒤에도 무진전 + 스크롤이 바닥 + scrollHeight 변화 50px 미만(미세 변동 실측 35px) → 예산을 다 쓰지 않고 반환(실측 3.1초, 예전엔 25초)
        if (a2 === b && window._atBottom() && Math.abs(document.body.scrollHeight - shB) < 50) { bottom = true; break; }
      }
    } else stuck = 0;
  }
  const after = Object.keys(window._all).length;
  return { collected: after, gained: after - before, blocked: Object.keys(window._blocked).length, rounds, wiggles, bottom,
    throttled, hidden: document.hidden,
    avgRoundMs: roundMs.length ? Math.round(roundMs.reduce((s, x) => s + x, 0) / roundMs.length) : null, lastRoundsMs: roundMs.slice(-3),
    avgWaitMs: waitMs.length ? Math.round(waitMs.reduce((s, x) => s + x, 0) / waitMs.length) : null, waitCapHits: waitMs.filter(w => w >= 250).length,
    authExpired, isLogin: /login|signin|auth/i.test(location.href),
    elapsedMs: Math.round(performance.now() - t0), scrollY: Math.round(scrollY) };
};

// ===== 준비 절차 =====
// 반환 JSON에 URL(쿼리 포함)을 담지 않는다 — 로그인 리다이렉트 URL의 쿼리스트링 때문에 결과 전체가 [BLOCKED]로 차단된다(2026-09-20 결함 2).
// shopIdOk는 pathname으로 본다 — 로그인 페이지의 returnUrl 안에도 shopId가 들어 있어 href 검색은 거기서도 true가 된다.
window._prepare = async function(shopId, name) {
  const d = await window._dismissAll();
  const bt = document.body.innerText;
  const r = { dismiss: d, shopIdOk: location.pathname.includes('/shops/' + shopId + '/'), titleOk: bt.includes(name),
    isLogin: /login|signin|auth/i.test(location.href), noShop: bt.includes('등록된 가게가 없'),
    path: location.host + location.pathname, hidden: document.hidden };
  if (r.isLogin || r.noShop || !r.titleOk) { r.stage = 'login'; return r; }
  r.filter = await window._applyPeriod('최근 30일', 30);
  window._all = {}; window._warn = []; window._blocked = {};
  window.scrollTo(0, 0); await new Promise(s => setTimeout(s, 300));
  r.ready = window._parse(); r.hidden = document.hidden; r.stage = 'ready';
  return r;
};
// 정의를 같은 오리진 localStorage에 저장 → 매장 2·3은 2-B 재주입판으로 되살린다. SID가 다르면 재주입하지 않는다(세션 간 재사용 금지).
window._saveDefs = function(sid) {
  try {
    const names = ['_dismiss','_dismissAll','_applyPeriod','_parse','_lost','_atBottom','_fp','_wait','_scroll','_prepare'];
    localStorage.setItem('_bm_defs', JSON.stringify({ sid, src: names.map(n => 'window.' + n + ' = ' + window[n].toString() + ';').join('\n') }));
    return true;
  } catch (e) { return false; }
};

// ===== 실행 =====
const _r = await window._prepare('<SHOP_ID>', '<매장명>');
_r.defsSaved = _r.stage === 'ready' ? window._saveDefs('<SID>') : false;   // 로그인 리다이렉트 중이면 저장하지 않는다(다른 오리진에 저장돼 쓸 수 없다)
JSON.stringify(_r)
```

### 2-B. 재주입판 (매장 2·3)

```javascript
const _d = JSON.parse(localStorage.getItem('_bm_defs') || 'null');
const _ok = !!_d && _d.sid === '<SID>';
if (_ok) eval(_d.src);
_ok ? JSON.stringify(await window._prepare('<SHOP_ID>', '<매장명>')) : 'NO_DEFS'
```

`NO_DEFS`가 나오면(다른 세션의 정의이거나 저장이 안 된 것) 그 매장은 **2-A 전체판**을 실행한다. 2-A는 항상 이 세션의 `SID`로 덮어쓴다. 로그인 리다이렉트 상태에서 2-A를 돌렸다면 정의는 `biz-member.baemin.com` 오리진에 저장돼 매장 페이지에서는 `NO_DEFS`가 된다 — 로그인 뒤 그 매장은 2-A를 다시 돌린다(2026-09-20 실측, 정상 동작).

**판정**

- 결과가 `[BLOCKED:`로 시작하면 → 로그인 리다이렉트 등으로 URL에 쿼리스트링이 생긴 것이다. **로그인 필요로 간주**하고 아래 로그인 문구로 요청한다. (정상 경로에서는 반환값에 URL이 없어 차단되지 않는다.)
- `stage: 'login'` (`isLogin` 또는 `noShop` 또는 `titleOk === false`) → **이 매장만 중단**하고 다른 매장은 계속한다. 사용자에게:
  > ⚠️ [매장명] 조회에 로그인이 필요합니다. 크롬에서 계정<A 또는 B>로 로그인 후 '완료했어요'라고 알려주세요.

  사용자 확인 후 이 매장의 Step 1(navigate)부터 재시도한다. 재시도해도 실패하면 **그 매장을 빼지 말고** 아래 형태로 결과 배열에 넣는다. 그래야 저장 단계가 그 매장 행을 보존한다.

  ```json
  {"storeName":"<매장명>","ok":false,"reason":"로그인 필요 — 계정<A 또는 B> 미로그인",
   "expectedTotal":null,"collected":0,"parseFail":0,"authExpired":true,
   "total":0,"dist":[0,0,0,0,0],"reviews":[]}
  ```
- `dismiss.gone === false` → `[WARN] 팝업이 계속 재등장함`만 남기고 계속 진행한다.
- `stage: 'ready'` → `filter`를 본다. 여기서 두 값을 기억한다. Step 4의 `ok` 판정이 이 둘을 쓴다.
  - **`filterOk`** — 기간 필터가 실제로 적용됐는가. `filter.ok && filter.filterOk`(다이얼로그 닫힘 + 기간 버튼의 두 날짜가 오늘−30일 ~ 오늘, ±1일)이면 `true`. `labelOk`는 참고용이다 — 라벨 문구만 바뀐 경우를 구분하려는 것이고 판정에는 쓰지 않는다.
  - **`expectedTotal`** — 페이지가 말하는 총건수. `0`과 `null`은 **다르다**.
- `filter.filterOk === false`(또는 `filter.ok === false`) → `window._applyPeriod('최근 30일', 30)`을 다시 호출한다(최대 2회). 단 `dialogOpen: true`(다이얼로그가 열린 채 적용이 안 된 상태)면 버튼을 다시 누르면 닫혀 버리므로 `window._applyPeriod('최근 30일', 30, true)`(열기 생략)로 호출한다. `stage: 'button_not_found'`·`'radio_not_found'`일 때만 `find(query="기간 필터 버튼")` / `find(query="최근 30일 라디오")`로 최후 fallback을 시도하고, ref를 받으면 `computer(action="left_click", ref=...)`로 클릭한 뒤 `_applyPeriod('최근 30일', 30, true)`를 호출한다. 재시도 후에도 `filterOk`가 false → `filterOk = false`, `[WARN] 기간 필터 미확인, 현재 필터로 진행`. 수집은 계속하되 **조회 범위가 불명이므로 이 매장은 `ok: false`다.** 범위를 모르는 결과로 "최근 30일 현황" 엑셀을 동기화하면 조회되지 않은 구간의 행이 지워진다.
- `expectedTotal === 0` → 해당 기간 리뷰 없음이 **확인된** 것. Step 3을 건너뛰고 이 매장은 `ok: true`, 0건으로 처리한다.
- `expectedTotal === null` → 표기 형식이 바뀐 것. 스크롤은 **정상 진행**하고 종료는 바닥 도달로만 판단한다. 0으로 간주해 건너뛰면 안 된다 — 리뷰를 놓치느니 느리게 끝까지 스크롤하는 쪽을 택한다. 다만 **수집률을 검증할 기준이 없으므로 이 매장은 `ok: false`(확인 필요)로 끝난다.** 수집한 저점수는 화면에 그대로 보고하되, 엑셀은 건드리지 않는다.
- `hidden`은 Step 3 시작 전에 본다(아래). 이 단계의 동작(팝업·필터·정의)은 숨김 상태에서도 정상이다.

> 헤더 계정명(예: "한종원님")은 두 계정이 같을 수 있으므로 **절대 계정명으로 로그인 여부를 판단하지 않는다.** URL의 shopId와 본문의 매장명으로만 확인한다.

> **좌표 확인용 스크린샷 금지.** 스크린샷을 찍을 때마다 뷰포트 크기가 미세하게 바뀌어 직전 좌표가 어긋나고, `Page.captureScreenshot` 타임아웃까지 누적된다. find()가 최후 수단으로 실패해도 화면을 보고 좌표를 클릭하지 말고 현재 필터로 진행한다.

---

## Step 3: hidden 게이트 + 스크롤 수집 (매장마다)

배민 리뷰 목록은 무한스크롤 + 윈도잉이라 DOM에는 화면 주변 카드만 유지된다(2026-09-09 실측: 최상단 6개, 스크롤 중 최대 15개 / 2026-09-20: 8~13개, 중앙값 12). 지속적으로 스크롤하며 그때그때 긁어모은다.

### 3-1. hidden 게이트 (스크롤 직전, 매장마다)

Step 2 반환의 `hidden`이 `true`면 **스크롤로 가지 않는다.** 크롬 창이 가려져 있어 타이머가 1초로 늘고 리스트가 로드되지 않는다(설계 근거 참고). 스크린샷·wait로는 풀리지 않으므로 사용자에게 아래 문구로 요청하고 **응답을 기다린다**:
> ⚠️ 크롬 창이 다른 창에 가려져 있어(document.hidden) 리뷰 목록이 로드되지 않습니다. 크롬 창을 화면 앞으로 꺼내 주시고(최소화 해제, Claude 앱에 가려지지 않게) '꺼냈어요'라고 알려주세요.

응답 후 `JSON.stringify({hidden: document.hidden})`으로 `false`를 확인하고 나서 3-2로 간다. 여전히 `true`면 같은 요청을 한 번 더 한다(최대 2회). 2회 후에도 `true`면 이 매장은 `ok: false`, `reason: "탭 숨김 — 크롬 창이 가려져 수집 불가"`로 결과 배열에 넣고 다음 매장으로 간다. 수집 도중 창이 다시 가려지는 경우는 3-2의 `throttled` 신호가 잡는다. `hidden`이 `false`면 바로 3-2.

### 3-2. 스크롤 실행

```javascript
JSON.stringify(await window._scroll(25000, <expectedTotal 또는 null>))
```

이 한 줄을 **target(= `collected + blocked` ≥ `expectedTotal`)에 도달하거나 `bottom: true`가 연속 2회 나올 때까지** 반복 호출한다. target 도달은 함수가 스스로 멈추므로 반환값의 `collected + blocked`를 `expectedTotal`과 비교하면 된다.

- 실측 기준(2026-09-20 수정 회차): 김치찜 175건(게시중단 1) 83라운드·6.9초(1회), 곱도리 87건 40라운드·3.9초(1회), 참 제육 91건 43라운드·4.2초(1회), 라운드당 83~98ms. 이 범위를 크게 벗어나면 먼저 `throttled`·`avgRoundMs`·`waitCapHits`를 본다.
- **`throttled === true`가 나오면 즉시 멈추고 사용자에게 요청한다.** 크롬 창이 수집 도중 가려진 것이다(라운드 3회 연속 600ms 초과, 실측 998/1001/1001ms·1129/1005/990ms). 스크린샷·wait로는 풀리지 않는다. 3-1과 같은 문구로 요청한다:
  > ⚠️ 크롬 창이 다른 창에 가려져 있어(document.hidden) 리뷰 목록이 로드되지 않습니다. 크롬 창을 화면 앞으로 꺼내 주시고(최소화 해제, Claude 앱에 가려지지 않게) '꺼냈어요'라고 알려주세요.

  응답 후 같은 `_scroll` 호출을 이어서 한다 — `window._all`은 유지되므로 처음부터 다시 할 필요 없다. `throttled` 반환은 호출 상한에 세지 않는다.
- `bottom: true`는 "wiggle 뒤에도 무진전이고 스크롤이 바닥이며 scrollHeight가 변하지 않은" 상태다(실측 3.1초에 반환). **연속 2회**면 종료한다. 1회로 종료하지 말 것 — 느린 로딩에서 한 배치를 통째로 헛돌 수 있다. `expectedTotal === null`이면 이것이 유일한 종료 근거다. 단 `throttled === true`인 반환은 종료 근거가 아니다.
- `blocked`는 건너뛴 게시중단 리뷰 수다. `collected + blocked`가 `expectedTotal`과 같으면 수집이 완전한 것이다(실측: 189 + 1 = 190, 173 + 1 = 174). Step 4가 이 합으로 `ok`를 판정한다.
- **`authExpired === true`가 나오면 즉시 중단한다.** 수집 도중 세션이 끊긴 것이므로 재시도해도 소용없다. 그 매장은 `ok: false`이고, 부분 수집분으로 엑셀을 동기화하면 안 된다. 사용자에게:
  > ⚠️ [매장명] 수집 도중 로그인이 풀렸습니다. 크롬에서 다시 로그인한 뒤 '완료했어요'라고 알려주시면 해당 매장만 다시 수집합니다.
- 최대 4회까지만 호출한다(`throttled` 반환 제외). 그 이상은 무한 루프로 본다. 25초 배치 하나가 약 270라운드·480,000px ≈ 카드 500건을 훑으므로 30일 리뷰는 보통 1회, 많아도 2회 + 바닥 확인으로 끝난다.
- `hidden === true`인데 `throttled === false`인 반환은 라운드 3회를 못 채우고 예산이 끝난 경우뿐이다. 다음 호출에서 `throttled`가 뜬다. **스크린샷·wait로 "깨우기"를 시도하지 않는다** — 2026-09-09 실측에서 둘 다 `hidden`을 풀지 못했고 프레임 1장만 강제해 카드 몇 개가 더 붙을 뿐이다.

---

## Step 4: 저점수 추출 (매장마다)

`<ET>`는 Step 2에서 기억한 `expectedTotal`(숫자 또는 `null`), `<FILTER_OK>`는 Step 2의 `filterOk`(`true`/`false`), `<AUTH>`는 Step 3의 마지막 `authExpired`(`true`/`false`)로 치환한다.

```javascript
const ET = <ET>, FILTER_OK = <FILTER_OK>, AUTH = <AUTH>;
const all = Object.values(window._all);
const parseFail = all.filter(r => r.stars < 1 || r.stars > 5).length;
const out = all.filter(r => (r.stars >= 1 && r.stars <= 3) || r.stars < 1 || r.stars > 5)
  .map(r => ({ ...r, reviewText: (r.stars < 1 || r.stars > 5 ? '[별점확인필요] ' : '') + r.reviewText }));
const blocked = Object.keys(window._blocked).length;
const sum = all.length + blocked;

// ok는 아래를 전부 통과할 때만 true. 하나라도 걸리면 저장 단계가 이 매장을 건드리지 않는다.
// 기본값은 실패다 — 판정을 통과해서 true가 되는 것이지, 오류가 없어서 true가 되는 게 아니다.
// 수집 완전성은 "수집 + 게시중단 ≥ 전체(N)"로 본다(2026-09-09·09-20 6/6 매장 정확 일치). 예전 98% 규칙은 1~3건 누락을 통과시켰다.
let ok = true, reason = '', warn = '';
if (AUTH)                                             { ok = false; reason = '인증만료 — 수집 도중 세션 종료'; }
else if (!FILTER_OK)                                  { ok = false; reason = '기간 필터 미확인 — 조회 범위 불명'; }
else if (ET === null)                                 { ok = false; reason = '총건수 확인 필요 — "전체(N)" 표기를 못 읽음'; }
else if (sum < ET)                                    { ok = false; reason = `누락 의심 — 수집 ${all.length} + 게시중단 ${blocked} < 전체 ${ET}`; }
else if (all.length > 0 && parseFail === all.length)  { ok = false; reason = '별점 필드 확인 필요 — 전건 파싱 실패'; }
else if (parseFail > all.length * 0.1)                { ok = false; reason = `별점 파싱 실패 ${parseFail}/${all.length} — 10% 초과`; }
if (ok && sum > ET) warn = `수집 ${all.length} + 게시중단 ${blocked} > 전체 ${ET} — 수집 중 신규 등록 추정`;   // ok 유지, 상태 열에 경고

JSON.stringify({ storeName: '<매장명>', ok, reason, warn,
  expectedTotal: ET, collected: all.length, total: all.length, authExpired: AUTH,
  blocked, countMatch: ET === null ? null : (sum === ET),
  dist: [1,2,3,4,5].map(n => all.filter(r => r.stars === n).length),
  parseFail, warns: window._warn.slice(0, 8), reviews: out })
```

**판정**

- `ok === true` → 저장 대상. 정상 수집이 확인된 매장이다.
- `ok === false` → **확인 필요.** 사용자에게 `reason`을 그대로 알리고 나머지 매장은 계속 진행한다. 이 매장은 저장 스크립트가 건너뛰므로 기존 엑셀 행이 지워지지 않는다. 수집된 저점수는 화면 보고에는 그대로 나열한다 — 엑셀에 안 들어갈 뿐 사용자가 못 보면 안 된다.
- `expectedTotal === 0`이고 `collected === 0` → `ok: true`. 그 기간에 리뷰가 정말 없는 것이다.
- `warn`이 비어 있지 않으면(수집 + 게시중단 > 전체) → `ok`는 유지한다. 스크롤 도중 새 리뷰가 달려 전체(N)을 읽은 시점보다 많아진 것으로 본다(2026-09-20 실측: 기준 측정 뒤 30분 사이 174→175). 최종 보고 표의 `상태` 열에 `warn`을 그대로 적는다.
- `parseFail > 0`인데 `ok === true`(10% 이하) → 일부 리뷰만 별점을 못 읽은 것이다. 진행하되 건수를 최종 보고에 올린다. 그 리뷰는 `[별점확인필요]`가 붙어 엑셀에 남는다.
- `누락 의심`(수집 + 게시중단 < 전체) → 재실행. 반복되면 바닥 도달 종료가 이른지, 전체(N)에 포함되는 다른 비노출 리뷰 유형이 생겼는지 배민 "차단" 탭 숫자와 대조한다(실측: `차단(1)` = blocked 1).

수집 단계에서는 모든 별점을 모으고 **여기서만** 저점수로 거른다. 수집 단계에서 미리 별점으로 걸러내면 페이지의 "전체(N)"과 비교해 스크롤 종료를 판단할 근거가 사라져, 하단의 저점수 리뷰를 놓칠 수 있다. 별점 파싱에 실패한 건도 저점수일 가능성을 배제할 수 없으므로 함께 보존한다.

저점수는 매우 희소해서(김치찜 최근 6개월 기준 1점 1건·2점 1건·3점 4건 — 2026-09-09 재확인; 최근 30일은 2026-09-09 3매장 0건, 2026-09-20 참 제육 1건) 이 JSON은 보통 아주 짧다. 반환값을 그대로 쓰면 되고 Blob·탭 이동은 필요 없다.

만약 반환값 끝에 `[TRUNCATED]` 표식이 붙어 잘렸으면(약 1,000자 초과) 그때만 폴백한다:
```javascript
const b = new Blob([JSON.stringify({storeName:'<매장명>', reviews: out})], {type:'text/plain;charset=utf-8'});
location.href = URL.createObjectURL(b); 'go'
```
이어서 `get_page_text(tabId=<탭ID>)`로 전문을 읽는다. 함정 세 가지: (1) `charset=utf-8`을 빼면 한글이 복구 불가능하게 깨진다. (2) 탭을 이동시키는 순간 `window._all`이 사라지므로 **그 매장 수집이 완전히 끝난 뒤에만** 실행한다. (3) 다른 탭에서 blob URL로 `navigate()`하는 방식은 동작하지 않는다 — 반드시 수집한 그 탭 자신을 이동시켜야 한다.

매장별 결과를 모아두고 다음 매장으로 넘어간다. **엑셀 저장은 3개 매장을 다 모은 뒤 한 번만 한다.**

---

## Step 5: 엑셀 저장 (전체 1회)

### 5-1. JSON 파일로 기록

`device_bash`로, 매장 3개 결과를 하나의 배열로 만들어 저장한다. **`ok === false`인 매장까지 그대로 포함한다** — 저장 스크립트가 `ok` 플래그를 보고 스스로 건너뛴다. 사람이 골라내지 않는다. 특수문자가 들어갈 수 있으므로 반드시 따옴표로 감싼 heredoc을 쓴다.

각 매장 객체에 최소한 `storeName` · `ok` · `reason` · `expectedTotal` · `collected` · `parseFail` · `reviews`가 들어가야 한다.

```bash
mkdir -p $HOME/skillwork && cat > $HOME/skillwork/baemin.json << 'ENDJSON'
[{"storeName":"...","ok":true,"reason":"","expectedTotal":0,"collected":0,"parseFail":0,"reviews":[]}, ...]
ENDJSON
```

`$HOME/skillwork`는 사용자에게 보이지 않는 작업 공간이다(세션이 끝나면 사라진다 — 백업을 여기 두지 않는다). 사용자 폴더에 임시 파일을 만들지 않는다.

### 5-2. 저장 스크립트

시트: **"전체" 단일 시트**. 열: `매장명 | 날짜 | 별점 | 리뷰번호 | 닉네임 | 주문메뉴 | 리뷰내용`. 날짜는 `YYYY-MM-DD`(쿠팡 스킬과 동일 형식). 정렬은 날짜 오름차순.

동기화 정책 — **`ok === true`인 매장에만 적용한다.** 그 매장의 행 중 이번 수집 결과에 리뷰번호가 없는 것은 삭제한다.

`ok !== true`인 매장의 행은 **읽지도 쓰지도 않는다.** 삭제도, 신규 추가도 하지 않고 그대로 둔다. 정상 수집된 매장이 하나도 없으면 파일을 열기만 하고 저장 없이 종료한다.

조회 기간 밖 과거 리뷰가 삭제되는 것은 의도된 동작이며, 이 엑셀은 "최근 30일 현황"이다. 되돌리기는 Step 0이 남긴 `backup/` 사본으로 한다.

저장 뒤 스크립트가 **파일을 다시 열어** 행수와 (매장명, 리뷰번호) 집합이 `kept + new`와 같은지 확인한다(`verified`). 점검·검증 회차에서는 두 번째 인자를 **엑셀 사본 경로**로 바꿔 돌리면(dry-run) 실제 파일을 건드리지 않고 같은 검증을 할 수 있다.

```bash
python3 - "$HOME/skillwork/baemin.json" "$HOME/mnt/claude/배민_저점수리뷰.xlsx" << 'ENDPY'
import json, sys, re, os
import openpyxl
from openpyxl.styles import Alignment, Font, PatternFill
from openpyxl.styles.numbers import FORMAT_TEXT

DATA, XLSX = sys.argv[1], sys.argv[2]
HEADER = ['매장명','날짜','별점','리뷰번호','닉네임','주문메뉴','리뷰내용']
WIDTHS = [24,14,10,20,16,36,60]

def star(n):
    try: n = int(n)
    except: return '?점'
    return '★'*n + '☆'*(5-n) if 1 <= n <= 5 else '?점'

def norm_date(d):
    """YYYY-MM-DD로 정규화. 예전 형식 'YYYY년 M월 D일'도 받아 자동 마이그레이션한다."""
    s = str(d or '').strip()
    m = re.match(r'(\d{4})-(\d{2})-(\d{2})$', s)
    if m: return s
    m = re.match(r'(\d{4})년\s*(\d{1,2})월\s*(\d{1,2})일', s)
    if m: return f'{m.group(1)}-{int(m.group(2)):02d}-{int(m.group(3)):02d}'
    return s

def sort_key(s):
    m = re.match(r'(\d{4})-(\d{2})-(\d{2})$', str(s or ''))
    return (int(m.group(1)), int(m.group(2)), int(m.group(3))) if m else (0,0,0)

with open(DATA, encoding='utf-8') as f:
    stores = json.load(f)

# ok가 정확히 True인 매장만 동기화 대상이다. 키가 없으면 실패로 본다(기본값 실패).
ok_stores = [s for s in stores if s.get('ok') is True]
skipped   = [s for s in stores if s.get('ok') is not True]
skip_names = {s['storeName'] for s in skipped}

if not ok_stores:
    print(json.dumps({'saved': False, 'reason': '정상 수집된 매장이 없어 저장하지 않음',
                      'skipped': [[s['storeName'], s.get('reason','')] for s in skipped]}, ensure_ascii=False))
    sys.exit(0)

try:
    wb = openpyxl.load_workbook(XLSX); print('[OK] 기존 엑셀 로드', file=sys.stderr)
except FileNotFoundError:
    wb = openpyxl.Workbook()
    if 'Sheet' in wb.sheetnames: del wb['Sheet']
    print('[OK] 새 엑셀 생성', file=sys.stderr)

if '전체' in wb.sheetnames:
    ws = wb['전체']
else:
    ws = wb.create_sheet('전체'); ws.append(HEADER)

rows = [list(r) for r in ws.iter_rows(min_row=2, values_only=True) if any(v not in (None,'') for v in r)]
for r in rows:
    r[1] = norm_date(r[1])

collected = {s['storeName']: {str(x['reviewNo']) for x in s.get('reviews', [])} for s in ok_stores}

kept, deleted, untouched = [], 0, 0
for r in rows:
    name, no = str(r[0] or '').strip(), str(r[3] or '').strip()
    if name in skip_names:    # 확인 필요 매장 — 손대지 않는다
        kept.append(r); untouched += 1; continue
    if name in collected and no not in collected[name]:
        deleted += 1          # 이번 수집 범위에서 사라진 행
    else:
        kept.append(r)        # 수집하지 않은 매장의 행은 그대로 보존

existing = {(str(r[0]).strip(), str(r[3]).strip()) for r in kept}
new = []
for s in ok_stores:
    for x in s.get('reviews', []):
        key = (s['storeName'], str(x['reviewNo']))
        if key in existing: continue
        existing.add(key)
        new.append([s['storeName'], norm_date(x.get('date')), star(x.get('stars')), str(x['reviewNo']),
                    x.get('nickname',''), x.get('menu',''), x.get('reviewText','')])

data = kept + new
data.sort(key=lambda r: sort_key(r[1]))

# 기존 데이터 영역을 실제로 삭제한다.
# 셀 값만 None으로 비우던 예전 방식은 빈 행이 남아 max_row가 계속 부풀었다(실측 575행 중 실데이터 1행).
if ws.max_row >= 2:
    ws.delete_rows(2, ws.max_row - 1)

ws.row_dimensions[1].height = 20.0
for i, w in enumerate(WIDTHS, start=1):
    ws.column_dimensions[chr(64+i)].width = w
fill = PatternFill(patternType='solid', fgColor='2F5496')
for c in ws[1]:
    c.font = Font(name='Arial', bold=True, color='FFFFFF')
    c.fill = fill
    c.alignment = Alignment(horizontal='center', vertical='center', wrap_text=True)

align = Alignment(horizontal='center', vertical='center', wrap_text=True)
for i, row in enumerate(data, start=2):
    ws.row_dimensions[i].height = 40.0
    for j, v in enumerate(row, start=1):
        c = ws.cell(row=i, column=j)
        if j == 4:
            c.value = str(v); c.number_format = FORMAT_TEXT   # 리뷰번호는 텍스트 강제
        else:
            c.value = v
        c.alignment = align; c.font = Font(bold=False)

try:
    wb.save(XLSX); saved = XLSX
except Exception as e:
    saved = os.path.expanduser('~/mnt/claude/backup/배민_저점수리뷰_backup.xlsx')
    os.makedirs(os.path.dirname(saved), exist_ok=True)
    wb.save(saved)
    print(f'[WARN] 원본 저장 실패 ({e}) → {saved}', file=sys.stderr)

# 저장 검증: 파일을 다시 열어 행수와 (매장명, 리뷰번호) 집합이 kept + new 와 같은지 확인한다.
expect_keys = {(str(r[0]).strip(), str(r[3]).strip()) for r in data}
wb2 = openpyxl.load_workbook(saved); ws2 = wb2['전체']
rows2 = [r for r in ws2.iter_rows(min_row=2, values_only=True) if any(v not in (None,'') for v in r)]
got_keys = {(str(r[0]).strip(), str(r[3]).strip()) for r in rows2}
verified = (len(rows2) == len(data)) and (got_keys == expect_keys)
if not verified:
    print(f'[WARN] 저장 검증 실패 — 파일 {len(rows2)}행 vs 기대 {len(data)}행, 키 차이 {len(got_keys ^ expect_keys)}', file=sys.stderr)

print(json.dumps({'saved': True, 'verified': verified, 'new': len(new), 'deleted': deleted, 'rows': len(data), 'file_rows': len(rows2),
                  'untouched_rows': untouched, 'synced': [s['storeName'] for s in ok_stores],
                  'skipped': [[s['storeName'], s.get('reason','')] for s in skipped],
                  'saved_to': saved, 'new_rows': new}, ensure_ascii=False))
ENDPY
```

- `saved: false` → 저장하지 않았다. `skipped` 사유를 그대로 사용자에게 알리고 재실행을 안내한다.
- 저장 성공 → `[OK] 엑셀 저장 (신규 N건, 삭제 N건, 총 N행, 검증 통과)`. `skipped`가 비어 있지 않으면 **반드시 함께 보고한다.**
- `verified: false` → 파일에 기록된 행이 기대와 다르다. 저장은 됐지만 **최종 보고에 `⚠️ 저장 검증 실패`를 올리고** 사용자가 파일을 열어 확인하도록 안내한다(`backup/`의 실행 전 사본과 비교).
- `saved_to`가 backup 경로면 → 사용자에게 엑셀을 닫고 다시 실행해달라고 알린다.

---

## Step 6: 정리

```
tabs_close_mcp(tabId=<탭ID>)
```

`localStorage`의 `_bm_defs`는 지우지 않아도 된다 — 다음 세션은 다른 `SID`라 재사용되지 않고, 매장 1이 덮어쓴다.

---

## 최종 보고 형식

```
조회 기간: YYYY-MM-DD ~ YYYY-MM-DD (최근 30일)

| 매장 | 전체(N) | 수집 | 게시중단 | 저점수 | 신규 | 별점분포(1~5) | 상태 |
|---|---|---|---|---|---|---|---|
| 김치찜의 정석 | 174건 | 173건 | 1건 | 0건 | 0건 | 0/0/0/1/172 | 정상 (173+1=174) |
| 퍽퍽살이 싫어 내가 만든 곱도리 | ... | | | | | | ⚠️ 확인필요: <reason> |
| 참 제육 | ... | | | | | | 정상 — 수집 91 + 게시중단 0 > 전체 90 (수집 중 신규 등록 추정) |

엑셀: 신규 N건 추가, N건 삭제, 총 N행, 검증 통과
```

- `전체(N)` 열은 `expectedTotal`을, `수집` 열은 `collected`를, `게시중단` 열은 `blocked`를 그대로 쓴다. **셋을 합쳐 쓰지 않는다** — 값이 벌어지는 것 자체가 신호다. `countMatch === true`면 상태 열에 `(수집+게시중단=전체)`처럼 합이 맞음을 적고, `warn`이 있으면 그 문구를 그대로 적는다. `expectedTotal`이 `null`이면 `확인필요`라고 적는다.
- `ok === false`인 매장은 상태 열에 `⚠️ 확인필요: <reason>`을 쓰고, **그 매장은 엑셀에 반영되지 않았음을 한 줄로 덧붙인다**("기존 행은 그대로 두었습니다"). `누락 의심`도 여기 올라온다 — 로그에만 남기지 않는다.
- `parseFail > 0`이면 표 아래에 `별점을 읽지 못한 리뷰 N건 — [별점확인필요] 표시로 저장됨`을 덧붙인다.
- `authExpired`가 있으면 재로그인 안내를 맨 위에 올린다.
- `verified: false`면 `⚠️ 저장 검증 실패`를 엑셀 줄에 적는다.
- 신규 저점수 리뷰가 있으면 그 내용을 매장별로 나열한다(리뷰내용이 `[배달리뷰] …`면 본문 없이 칩만 있는 리뷰, `[파트너전용] …`이면 비공개 리뷰다). 없으면 "신규 저점수 리뷰 없음"이라고만 쓴다. 단 `ok === false`인 매장이 있으면 "없음"이라고 단정하지 말고 **"확인 필요"**라고 쓴다.

---

## 트러블슈팅

| 증상 | 원인 | 대응 |
|---|---|---|
| 리뷰가 있는데 1건만 수집하고 끝남 | `전체(1,460)`처럼 쉼표가 든 숫자를 `\d+`로 읽어 1로 오인 | Step 2 `_applyPeriod`의 정규식은 `([\d,]+)` + 쉼표 제거. 절대 되돌리지 말 것 |
| 리뷰가 있는데 스크롤 없이 종료 | "전체(N)" 표기 변경으로 파싱 실패 | `expectedTotal`을 0이 아닌 `null`로 두고 바닥 도달로만 종료 |
| Step 2 결과가 `[BLOCKED: Cookie/query string data]` | 로그인 리다이렉트(`biz-member.baemin.com/login?returnUrl=…&__ts=…`) 등으로 URL에 쿼리스트링이 생겼는데 반환값에 URL이 들어감 | 로그인 필요로 간주하고 로그인 요청. 반환 JSON에 `location.href`를 넣지 말 것(`path`만). 2026-09-20 실측 |
| Step 2 결과가 `NO_DEFS` | 재주입 키 `SID`가 다르거나(다른 세션) 저장이 안 됨 | 그 매장은 2-A 전체판으로 실행 |
| `filterOk: false`인데 `labelOk: true` | 다이얼로그가 열린 채 남아 라벨은 보이지만 적용이 안 됨(2026-09-20 재현) | 적용된 것이 아니다. `_applyPeriod` 재시도. 되돌려서 `labelOk`로 판정하지 말 것 |
| `filterOk: false`, `range`가 오늘−30일~오늘과 하루 차이 | 자정 경계 | ±1일은 통과 조건이다. 이틀 이상 벌어지면 필터 미적용 |
| 별점이 44 같은 이상값 | SVG 색상 세기 fallback이 병합 카드나 새 노란 아이콘을 세었다 | 현재 사이트에는 `aria-label`이 없어 **색상 경로가 실제 주 경로**다(2026-09-09 381/381, 2026-09-20 349/349). 이상값은 `c <= 5` 가드로 버려져 `star_fail`로 잡힌다. `merged` 경고와 카드 안 svg 목록(별 5개 16px + 아이콘 12px)을 확인할 것. aria-label 경로는 사이트가 되돌아올 때 대비용이므로 지우지 말 것 |
| 리뷰번호는 맞는데 내용이 뒤섞임 | 카드 경계 오탐 | `ReviewContent-module` 경계 실패 → `no_card`/`merged` 경고 확인 |
| 리뷰내용에 본문이 두 번 들어감 / `보이는 리뷰입니다.`가 섞임 | 파트너전용 라벨 줄을 비공개 구간 머리말로 오인 | `PK_LABEL` 플래그 경로가 살아 있는지 확인(2026-09-20 결함 3). `[파트너전용] 본문` 한 번만 들어가야 정상 |
| `nick_fail`인데 카드에 닉네임이 있음 | 닉네임에 메타 단어(`주문` 등)가 포함 | `META`는 정확 패턴만 쓴다. 부분 일치 금지어로 되돌리지 말 것(2026-09-20 결함 4: `내가주문한`) |
| 리뷰내용이 `[배달리뷰] 좋아요`처럼 칩만 있음 | 본문 없는 리뷰(30% 안팎) | 정상. 칩은 본문이 없을 때만 기록된다 |
| 필터 클릭이 먹히지 않음 | 프로모션 팝업이 클릭을 가로채 | `_dismissAll()`이 선행되는지 확인 |
| 팝업 WARN이 뜨는데 실제 팝업은 없음 | 우측 하단 챗봇 위젯을 팝업으로 오탐 | `isChatbot` 제외가 살아있는지 확인 |
| `Failed to fetch (self-api.baemin.com)` | 크로스 오리진 차단 (정상) | API 경로를 되살리려 하지 말 것. 위 "설계 근거" 참고 |
| CDP 타임아웃 45초 | 배치 예산이 45초에 근접 | `_scroll` 예산을 25초 이하로 유지 |
| 결과 끝에 `[TRUNCATED]` | `javascript_tool` 반환 약 1,000자 제한 | Step 4의 Blob 폴백. 수집 결과 JSON은 보통 그보다 짧다 |
| 라운드가 ~1000ms로 느려짐 (`throttled: true`, `avgRoundMs` ≈ 870~1000) | **감속** — 크롬 창이 가려져(`hidden: true`) 백그라운드 타이머 클램프로 `setTimeout`이 1초에 깨어남(실측 998/1001/1001ms, 1129/1005/990ms). 적응형 대기의 25ms 폴도 같이 1초가 된다 | 스크린샷·wait로는 안 풀린다. 사용자에게 크롬 창을 앞으로 꺼내달라고 요청하고 응답 후 `hidden === false` 확인, 같은 `_scroll`을 이어서 호출 |
| 스크롤해도 `gained: 0`, scrollHeight가 안 늘어남, `hidden: true` | **정지** — 같은 원인으로 렌더링이 멈춰 rAF가 오지 않고(실측 2초 내 0회) 무한스크롤 로더가 돌지 않음. `computer` 호출은 프레임 1장만 강제해 카드 몇 개가 붙을 뿐 | 위와 같음. `gained: 0`을 "끝"으로 오판하지 말 것 — `throttled`가 함께 true면 종료 근거가 아니다 |
| Step 2 결과 `hidden: true` | 새 창이 다른 창(보통 Claude 앱)에 가려진 채 열림 — 기본값에 가깝다. 사용자가 Claude 앱에서 답장을 쓰는 동안에도 재발 | Step 3-1 게이트: 창을 꺼내달라고 요청, `hidden === false` 확인 후 스크롤 |
| `bottom: true`인데 `collected + blocked < expectedTotal` | 로더 지연 중 바닥으로 판정했거나 진짜 누락 | 같은 `_scroll`을 한 번 더 호출(연속 2회 규칙). 두 번째도 `bottom`이면 Step 4가 `누락 의심`으로 `ok: false` |
| `waitCapHits`가 라운드 수에 가까움 | 지문이 안 바뀜 — 사이트가 스크롤에 반응하지 않거나 지문 대상(리뷰번호 span)이 바뀜 | `_fp`의 리뷰번호 정규식과 DOM을 확인. 이 경우 라운드는 예전 고정 250ms와 같아져 느려질 뿐 결과는 같다 |
| 저장은 됐는데 파일이 계속 커짐 | 빈 행 누적 | `delete_rows`를 쓰는지 확인 (셀 None 비우기 금지) |
| `verified: false` | 저장 후 재열기 결과가 기대 행·키와 다름 | 파일을 열어 확인. `backup/`의 실행 전 사본과 비교 |
| "등록된 가게가 없어요" | 계정 불일치 | 해당 매장 계정으로 로그인 후 재시도 |
| 한 매장의 저점수가 통째로 사라짐 | 실패를 모르고 빈 `reviews`로 동기화함 | `ok` 플래그가 살아 있는지 확인. 저장 스크립트의 `ok_stores` 필터를 제거하지 말 것 |
| `총건수 확인 필요` (ok:false) | "전체(N)" 표기 형식 변경 | Step 2의 정규식 `전체\s*\(([\d,]+)\)` 확인. **`null`을 `0`으로 되돌리지 말 것** — 기존 엑셀 행이 지워진다 |
| `기간 필터 미확인` (ok:false) | 다이얼로그 구조 변경으로 필터 적용 실패, 또는 적용이 무효 | 조회 범위를 모르는 채 동기화하면 안 된다. 필터를 고친 뒤 재실행 |
| `인증만료 — 수집 도중 세션 종료` | 스크롤 중 세션 만료 | 그 매장은 실패 처리되어 엑셀이 보존된다. 재로그인 후 그 매장만 재수집 |
| `누락 의심 — 수집 N + 게시중단 M < 전체 K` | 스크롤이 끝까지 못 갔거나, 전체(N)에 포함되는 다른 비노출 리뷰 유형 | 재실행. 반복되면 배민 "차단" 탭 숫자와 대조(실측: `차단(1)` = blocked 1) |
| `별점 파싱 실패 N/M — 10% 초과` (ok:false) | 별점 UI가 바뀌어 일부만 읽힘 | 카드 안 svg·aria-label·img alt 구조를 다시 확인. 10% 이하면 `[별점확인필요]`로 보존하며 진행 |
| 게시중단된 리뷰가 결과에 안 보임 | 의도적 제외 (우리가 신고해 내린 리뷰) | 정상. `blocked` 건수로 보고되며 배민 "차단" 탭 숫자와 같아야 한다(실측 2026-09-09: blocked 1 = `차단(1)`). 30일 차단이 풀리면 정상 수집 대상으로 돌아온다 |
| `수집 + 게시중단 > 전체` (warn) | 전체(N)을 읽은 뒤 스크롤 중 새 리뷰가 달림 | `ok` 유지, 상태 열에 경고만. 다음 실행에서 사라진다 |
