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
- **스크린샷으로 탭을 "깨우지" 않는 이유**: 이 환경에서 탭은 `document.hidden === false`이고 rAF 스로틀링이 없다. 순수 JS 스크롤만으로 라운드당 약 250ms가 나온다(실측: 205건을 약 50초, 도구 호출 2회). 스크린샷 깨우기·browser_batch 사이클은 불필요한 왕복이므로 쓰지 않는다. 단 Step 4에서 `hidden === true`가 확인되면 그때만 예외적으로 스크린샷을 끼워 넣는다.
- **`javascript_tool` 호출은 45초에서 CDP 타임아웃**이 난다. 배치 예산은 25초를 넘기지 않는다.
- **엑셀은 사용자 PC에서 직접 처리**한다(`device_bash`). 클라우드 컨테이너엔 `/sessions` 경로 자체가 없으므로 `find /sessions/*/mnt/...` 같은 탐색을 되살리지 말 것. 스테이징·전송·커밋 왕복도 필요 없다.

매장 처리가 실패해도 다음 매장은 계속 진행한다. 전체를 중단하는 조건은 Step 0(환경 확인 실패)뿐이다.

### 조용한 0건을 막는 세 가지 원칙 (2026-09-06 도입 — 되돌리지 말 것)

이 스킬의 가장 위험한 실패는 "조용히 0건"이다. DOM 스크롤이 중간에 끊겼는지, 리뷰가 정말 없는지 구분하지 못하면 **"저점수 리뷰 없음"이라는 잘못된 안심**을 주고, 이어지는 저장 단계가 기존 엑셀 행까지 지운다. 아래 셋은 그 사고를 막는 장치다.

1. **`expectedTotal`은 `0`과 `null`이 다르다.** `0`은 "그 기간에 리뷰가 없음이 확인됨"이고, `null`은 "총건수를 못 읽음 = 확인 필요"다. `null`에 `|| 0`을 붙이지 말 것. `null`인 매장은 스크롤은 끝까지 하되 **저장 단계에서 읽지도 쓰지도 않는다**(기존 행 보존).
2. **별점을 못 읽은 리뷰를 조용히 버리지 않는다.** `parseFail`로 세고, `[별점확인필요]`를 붙여 저점수 목록에 남기며, 최종 보고에 건수를 올린다. 4~5점으로 **확인된** 리뷰만 제외한다.
3. **매장 성공/실패는 `ok` 플래그로 판정한다.** `ok: true`는 "기간 필터가 적용됐고 · 총건수를 읽었고 · 수집률이 98% 이상이고 · 수집 도중 세션이 끊기지 않았다"를 전부 만족할 때만 붙는다. 기본값은 실패다. 저장 스크립트는 `ok !== true`인 매장을 통째로 건너뛴다.

> 실패한 매장도 **JSON 배열에서 빼지 않는다.** `ok: false`인 채로 그대로 넣는다. 저장 스크립트가 `ok`를 보고 스스로 건너뛴다 — 사람이 골라내지 않는다.

---

## Step 0: 환경 확인

`device_bash`로 폴더·파이썬·잠금파일을 한 번에 확인한다.

```bash
ls -d $HOME/mnt/claude && python3 -c "import openpyxl;print('openpyxl ok')" && ls $HOME/mnt/claude/'~$배민_저점수리뷰.xlsx' 2>/dev/null && echo LOCKED || echo UNLOCKED
```

- `LOCKED` → 사용자에게 엑셀을 닫아달라고 요청하고 재확인한다.
- `device_bash` 자체가 실패하면 → 사용자에게 "PC에 연결되어 있지 않아 엑셀 저장을 할 수 없다"고 알리고, 수집은 진행하되 결과를 채팅에 표로만 출력한다.

엑셀 경로는 `$HOME/mnt/claude/배민_저점수리뷰.xlsx` 이다.

---

## Step 1: 탭 열기 (최초 1회)

```
tabs_context_mcp(createIfEmpty=true)
navigate(url=https://self.baemin.com/shops/14698107/reviews, tabId=<탭ID>)
```

매장이 바뀔 때는 같은 탭에서 `navigate`만 다시 한다. 페이지가 이동하면 `window`의 모든 상태가 초기화되므로 **매 매장마다 Step 2~4의 스크립트를 처음부터 다시 실행**한다.

---

## Step 2: 팝업 닫기 + 로그인 확인 (매장마다, 1회 호출)

`<SHOP_ID>`, `<매장명>`은 실제 값으로 치환한다.

```javascript
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
const _d = await window._dismissAll();
JSON.stringify({ dismiss: _d, shopIdOk: location.href.includes('<SHOP_ID>'),
  titleOk: document.body.innerText.includes('<매장명>'),
  isLogin: /login|signin|auth/i.test(location.href),
  noShop: document.body.innerText.includes('등록된 가게가 없'),
  url: location.href, hidden: document.hidden })
```

**판정**

- `shopIdOk && titleOk` 이면 정상 → Step 3.
- `isLogin` 또는 `noShop` 또는 `titleOk === false` → **이 매장만 중단**하고 다른 매장은 계속한다. 사용자에게:
  > ⚠️ [매장명] 조회에 로그인이 필요합니다. 크롬에서 계정<A 또는 B>로 로그인 후 '완료했어요'라고 알려주세요.

  사용자 확인 후 이 매장의 Step 1(navigate)부터 재시도한다. 재시도해도 실패하면 **그 매장을 빼지 말고** 아래 형태로 결과 배열에 넣는다. 그래야 저장 단계가 그 매장 행을 보존한다.

  ```json
  {"storeName":"<매장명>","ok":false,"reason":"로그인 필요 — 계정<A 또는 B> 미로그인",
   "expectedTotal":null,"collected":0,"parseFail":0,"authExpired":true,
   "total":0,"dist":[0,0,0,0,0],"reviews":[]}
  ```
- `gone === false` → `[WARN] 팝업이 계속 재등장함`만 남기고 계속 진행한다.
- `hidden === true` → 이 값을 기억해 둔다. Step 4의 스크롤이 정체될 때만 쓴다.

> 헤더 계정명(예: "한종원님")은 두 계정이 같을 수 있으므로 **절대 계정명으로 로그인 여부를 판단하지 않는다.** URL의 shopId와 본문의 매장명으로만 확인한다.

---

## Step 3: 기간 필터 "최근 30일" 적용 (매장마다, 1회 호출)

`find()`나 스크린샷을 쓰지 않는다. 다이얼로그 오픈은 최대 3초 폴링해 콜드 스타트 지연을 흡수한다.

```javascript
window._applyPeriod = async function(label = '최근 30일', skipOpen = false, maxWait = 3000) {
  await window._dismissAll();   // 팝업이 열려 있으면 필터 버튼 클릭을 가로채서 엉뚱한 기간이 선택된다
  const inView = el => { const r = el.getBoundingClientRect(); return r.width > 0 && r.height > 0; };
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
  const isOpen = () => [...document.querySelectorAll('[role="dialog"],[aria-modal="true"]')]
    .some(e => inView(e) && /기간/.test(e.innerText || '') && /최근\s*\d+\s*(일|개월)/.test(e.innerText || ''));
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
  if (apply) apply.click(); else return { ok: false, stage: 'apply_not_found', waited };

  await new Promise(s => setTimeout(s, 2500));
  // 전체(N) — 쉼표 포함 숫자를 반드시 처리한다. "전체(1,460)"에서 \d+ 만 쓰면 1로 읽혀 수집이 1건에서 끝난다.
  const m = document.body.innerText.match(/전체\s*\(([\d,]+)\)/);
  return { ok: true, waited,
    labelOk: document.body.innerText.includes(label),
    range: document.body.innerText.match(/\d{4}\.\s*\d{1,2}\.\s*\d{1,2}[^~]*~[^\n]*/)?.[0] || null,
    expectedTotal: m ? parseInt(m[1].replace(/,/g, ''), 10) : null };
};
JSON.stringify(await window._applyPeriod('최근 30일'))
```

**판정**

여기서 두 값을 기억한다. Step 4-3의 `ok` 판정이 이 둘을 쓴다.

- **`filterOk`** — 기간 필터가 실제로 적용됐는가. `ok && (labelOk || range가 실제 30일 범위)`이면 `true`.
- **`expectedTotal`** — 페이지가 말하는 총건수. `0`과 `null`은 **다르다**.

판정:

- `ok && labelOk` → `filterOk = true`. `expectedTotal`을 기억하고 Step 4로. `labelOk`가 false여도 `range`가 실제 30일 범위면 정상으로 본다(라벨 문구만 바뀌는 경우).
- `expectedTotal === 0` → 해당 기간 리뷰 없음이 **확인된** 것. 스크롤을 건너뛰고 이 매장은 `ok: true`, 0건으로 처리한다.
- `expectedTotal === null` → 표기 형식이 바뀐 것. 스크롤은 **정상 진행**하고 종료는 무진전 감지로만 판단한다. 0으로 간주해 건너뛰면 안 된다 — 리뷰를 놓치느니 느리게 끝까지 스크롤하는 쪽을 택한다. 다만 **수집률을 검증할 기준이 없으므로 이 매장은 `ok: false`(확인 필요)로 끝난다.** 수집한 저점수는 화면에 그대로 보고하되, 엑셀은 건드리지 않는다.
- `stage: 'dialog_not_opened'` → `_applyPeriod()`를 처음부터 다시 호출한다(최대 2회).
- `stage: 'button_not_found'` 또는 `'radio_not_found'` → 이때만 `find(query="기간 필터 버튼")` / `find(query="최근 30일 라디오")`로 최후 fallback을 시도하고, ref를 받으면 `computer(action="left_click", ref=...)`로 클릭한 뒤 `_applyPeriod('최근 30일', true)`를 호출한다.
- 2회 재시도 후에도 실패 → `filterOk = false`. `[WARN] 기간 필터 실패, 현재 필터로 진행`. 수집은 계속하되 **조회 범위가 불명이므로 이 매장은 `ok: false`다.** 범위를 모르는 결과로 "최근 30일 현황" 엑셀을 동기화하면 조회되지 않은 구간의 행이 지워진다.

> **좌표 확인용 스크린샷 금지.** 스크린샷을 찍을 때마다 뷰포트 크기가 미세하게 바뀌어 직전 좌표가 어긋나고, `Page.captureScreenshot` 타임아웃까지 누적된다. find()가 최후 수단으로 실패해도 화면을 보고 좌표를 클릭하지 말고 현재 필터로 진행한다.

---

## Step 4: 스크롤 수집 (매장마다)

배민 리뷰 목록은 virtual scroll이라 DOM에는 화면에 보이는 약 6개만 유지된다. 지속적으로 스크롤하며 그때그때 긁어모은다.

### 4-1. 파서 + 스크롤 함수 정의 (1회 호출)

```javascript
window._all = {}; window._warn = [];
window._parse = function() {
  const DT = ['가게배달','한집배달','알뜰배달','배달','포장','직접배달','배민배달','가게포장'];
  const PK = ['사장님께만 보이는','파트너님에게만','파트너에게만','비공개 리뷰','점주에게만'];
  for (const s of document.querySelectorAll('span')) {
    const t = s.innerText?.trim(); if (!t) continue;
    const m = t.match(/^리뷰번호\s+(\d+)$/) || t.match(/^(20\d{14})$/);
    if (!m || window._all[m[1]]) continue;
    const no = m[1];
    // 카드 경계는 이 클래스가 정상·특수 리뷰 모두에서 단일 리뷰만 정확히 감싼다(실측 182/182).
    // 부모로 walk-up하는 예전 방식은 이웃 리뷰까지 병합해 데이터를 섞어버렸다 — 되살리지 말 것.
    const card = s.closest('[class*="ReviewContent-module"]');
    if (!card) { window._warn.push('no_card:' + no); continue; }
    if (card.innerText?.includes('게시중단 요청으로 인해')) continue;   // 우리가 신고해 내린 리뷰 — 수집 제외
    if ((card.innerText.match(/리뷰번호/g) || []).length > 1) { window._warn.push('merged:' + no); continue; }

    const text = card.innerText || '';
    // 별점: aria-label이 가장 정확하다(실측 100%). SVG 색상 세기는 44같은 이상값을 만든 전력이 있어 fallback으로만 쓴다.
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
    const okNick = n => n && n.length < 30 && !n.startsWith('(') && !/주문|리뷰번호|배달리뷰|답글|사장님/.test(n) && !DT.includes(n);
    let nick = '';
    const di = lines.findIndex(l => DT.includes(l));
    if (di >= 0 && okNick(lines[di + 1])) nick = lines[di + 1];
    if (!nick) { const oi = lines.findIndex(l => /^\d+회\s*주문\s*고객$/.test(l)); if (oi > 0 && okNick(lines[oi - 1])) nick = lines[oi - 1]; }
    if (!nick && date) { const li = lines.findIndex(l => l.includes(dm[1])); if (li > 0 && okNick(lines[li - 1])) nick = lines[li - 1]; }

    const menu = (text.match(/주문메뉴\s*\n?\s*([^\n]+)/) || [])[1]?.trim() || '';

    const ms = text.indexOf('주문메뉴'), di2 = dm ? text.indexOf(dm[0]) : -1;
    let pub = '';
    if (ms > 0 && di2 >= 0) {
      pub = text.substring(di2, ms).split('\n').map(x => x.trim())
        .filter(l => l && !/^\d{4}년/.test(l) && !l.includes('리뷰번호') && !l.includes('주문 고객')
          && !l.includes('누적 주문') && !DT.some(d => l.includes(d)) && !PK.some(k => l.includes(k)))
        .join(' ').trim();
    }
    let partner = '';
    for (const kw of PK) {
      const ki = text.indexOf(kw); if (ki < 0) continue;
      const after = text.substring(ki + kw.length);
      let stop = after.length;
      for (const sp of ['주문메뉴','배달리뷰','답글']) { const si = after.indexOf(sp); if (si >= 0 && si < stop) stop = si; }
      const pl = after.substring(0, stop).split('\n').map(x => x.trim()).filter(l => l && l.length > 1);
      if (pl.length) { partner = pl.slice(0, 5).join(' ').trim(); break; }
    }
    let tag = '';   // 자유 텍스트 없이 태그 칩만 있는 리뷰
    if (ms <= 0) {
      const dr = lines.indexOf('배달리뷰');
      if (dr >= 0) { const a = []; for (let i = dr + 1; i < lines.length; i++) { if (/^사장님|^\d{4}년/.test(lines[i])) break; a.push(lines[i]); } tag = a.join(', '); }
    }
    let review = '(텍스트 없음)';
    if (pub && partner) review = `${pub} / [파트너전용] ${partner}`;
    else if (pub) review = pub;
    else if (partner) review = `[파트너전용] ${partner}`;
    else if (tag) review = `[태그만 있는 리뷰] ${tag}`;

    if (!nick) window._warn.push('nick_fail:' + no);
    if (!menu && ms > 0) window._warn.push('menu_fail:' + no);
    window._all[no] = { reviewNo: no, stars, date, nickname: nick, menu, reviewText: review };
  }
  return Object.keys(window._all).length;
};

// 배치 예산 25초 (CDP 타임아웃 45초보다 충분히 아래).
// 수집 도중 세션이 끊기면 DOM이 비어 gained===0 → "정상 종료"처럼 보인다.
// 매 라운드 로그인 상태를 확인해 치명 실패로 올린다. (쿠팡의 401 fatal에 대응하는 장치)
window._lost = () => /login|signin|auth/i.test(location.href)
  || document.body.innerText.includes('등록된 가게가 없');
window._scroll = async function(budgetMs = 25000, target = null) {
  const t0 = performance.now(); let rounds = 0, stuck = 0, authExpired = false;
  const before = Object.keys(window._all).length;
  while (performance.now() - t0 < budgetMs) {
    if (window._lost()) { authExpired = true; break; }   // 즉시 중단. 재시도해도 소용없다
    rounds++;
    const b = Object.keys(window._all).length;
    window.scrollBy(0, 900);
    await new Promise(r => setTimeout(r, 250));
    const a = window._parse();
    if (target && a >= target) break;
    if (a === b) {
      if (++stuck >= 3) {   // wiggle: 위로 살짝 올렸다 크게 내려 lazy-load 재유도
        await window._dismissAll();
        window.scrollBy(0, -600); await new Promise(r => setTimeout(r, 200));
        window.scrollBy(0, 1500); await new Promise(r => setTimeout(r, 600));
        window._parse(); stuck = 0;
      }
    } else stuck = 0;
  }
  const after = Object.keys(window._all).length;
  return { collected: after, gained: after - before, rounds,
    authExpired, isLogin: /login|signin|auth/i.test(location.href),
    elapsedMs: Math.round(performance.now() - t0), scrollY: Math.round(scrollY), hidden: document.hidden };
};
window.scrollTo(0, 0);
'ready:' + window._parse()
```

### 4-2. 스크롤 실행

```javascript
JSON.stringify(await window._scroll(25000, <expectedTotal 또는 null>))
```

이 한 줄을 **`collected`가 `expectedTotal`에 도달하거나 `gained === 0`이 연속 2회 나올 때까지** 반복 호출한다.

- 실측 기준: 205건이 약 50초(호출 2회). 이 범위를 크게 벗어나면 사이트 지연을 의심한다.
- `gained === 0`이 **연속 2회** 나오면 종료한다. 1회로 종료하지 말 것 — 느린 로딩에서 한 배치를 통째로 헛돌 수 있다.
- **`authExpired === true`가 나오면 즉시 중단한다.** 수집 도중 세션이 끊긴 것이므로 재시도해도 소용없다. 그 매장은 `ok: false`이고, 부분 수집분으로 엑셀을 동기화하면 안 된다. 사용자에게:
  > ⚠️ [매장명] 수집 도중 로그인이 풀렸습니다. 크롬에서 다시 로그인한 뒤 '완료했어요'라고 알려주시면 해당 매장만 다시 수집합니다.
- 수집률은 Step 4-3의 `ok` 판정에서 한 번에 따진다. `collected`가 `expectedTotal`의 98% 이상이면 정상으로 본다(차단·비노출 리뷰가 전체(N)에 포함될 수 있다). 98% 미만이면 `ok: false`이며, **로그에만 남기지 않고 최종 보고 표의 `상태` 열에 올린다.**
- 최대 8회까지만 호출한다. 그 이상은 무한 루프로 본다.
- **`hidden === true`가 반환될 때만** — 이 환경에서는 정상이면 나오지 않는다 — 다음 호출 직전에 `computer(action="screenshot", tabId=<탭ID>)`를 한 번 끼워 넣어 렌더링을 재개시킨다. `hidden === false`이면 스크린샷을 찍지 않는다.

### 4-3. 저점수 추출

`<ET>`는 Step 3에서 기억한 `expectedTotal`(숫자 또는 `null`), `<FILTER_OK>`는 Step 3의 `filterOk`(`true`/`false`), `<AUTH>`는 Step 4-2의 마지막 `authExpired`(`true`/`false`)로 치환한다.

```javascript
const ET = <ET>, FILTER_OK = <FILTER_OK>, AUTH = <AUTH>;
const all = Object.values(window._all);
const parseFail = all.filter(r => r.stars < 1 || r.stars > 5).length;
const out = all.filter(r => (r.stars >= 1 && r.stars <= 3) || r.stars < 1 || r.stars > 5)
  .map(r => ({ ...r, reviewText: (r.stars < 1 || r.stars > 5 ? '[별점확인필요] ' : '') + r.reviewText }));

// ok는 아래를 전부 통과할 때만 true. 하나라도 걸리면 저장 단계가 이 매장을 건드리지 않는다.
// 기본값은 실패다 — 판정을 통과해서 true가 되는 것이지, 오류가 없어서 true가 되는 게 아니다.
let ok = true, reason = '';
if (AUTH)                                             { ok = false; reason = '인증만료 — 수집 도중 세션 종료'; }
else if (!FILTER_OK)                                  { ok = false; reason = '기간 필터 미확인 — 조회 범위 불명'; }
else if (ET === null)                                 { ok = false; reason = '총건수 확인 필요 — "전체(N)" 표기를 못 읽음'; }
else if (ET > 0 && all.length < ET * 0.98)            { ok = false; reason = `수집률 낮음 ${all.length}/${ET}`; }
else if (all.length > 0 && parseFail === all.length)  { ok = false; reason = '별점 필드 확인 필요 — 전건 파싱 실패'; }

JSON.stringify({ storeName: '<매장명>', ok, reason,
  expectedTotal: ET, collected: all.length, total: all.length, authExpired: AUTH,
  dist: [1,2,3,4,5].map(n => all.filter(r => r.stars === n).length),
  parseFail, warns: window._warn.slice(0, 8), reviews: out })
```

**판정**

- `ok === true` → 저장 대상. 정상 수집이 확인된 매장이다.
- `ok === false` → **확인 필요.** 사용자에게 `reason`을 그대로 알리고 나머지 매장은 계속 진행한다. 이 매장은 저장 스크립트가 건너뛰므로 기존 엑셀 행이 지워지지 않는다. 수집된 저점수는 화면 보고에는 그대로 나열한다 — 엑셀에 안 들어갈 뿐 사용자가 못 보면 안 된다.
- `expectedTotal === 0`이고 `collected === 0` → `ok: true`. 그 기간에 리뷰가 정말 없는 것이다.
- `parseFail > 0`인데 `ok === true` → 일부 리뷰만 별점을 못 읽은 것이다. 진행하되 건수를 최종 보고에 올린다. 그 리뷰는 `[별점확인필요]`가 붙어 엑셀에 남는다.

수집 단계에서는 모든 별점을 모으고 **여기서만** 저점수로 거른다. 수집 단계에서 미리 별점으로 걸러내면 페이지의 "전체(N)"과 비교해 스크롤 종료를 판단할 근거가 사라져, 하단의 저점수 리뷰를 놓칠 수 있다. 별점 파싱에 실패한 건도 저점수일 가능성을 배제할 수 없으므로 함께 보존한다.

저점수는 매우 희소해서(김치찜 최근 6개월 기준 1점 1건·2점 1건·3점 4건) 이 JSON은 보통 아주 짧다. 반환값을 그대로 쓰면 되고 Blob·탭 이동은 필요 없다.

만약 반환값이 잘린 것으로 보이면(끝이 잘린 JSON) 그때만 폴백한다:
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

`$HOME/skillwork`는 사용자에게 보이지 않는 작업 공간이다. 사용자 폴더에 임시 파일을 만들지 않는다.

### 5-2. 저장 스크립트

시트: **"전체" 단일 시트**. 열: `매장명 | 날짜 | 별점 | 리뷰번호 | 닉네임 | 주문메뉴 | 리뷰내용`. 날짜는 `YYYY-MM-DD`(쿠팡 스킬과 동일 형식). 정렬은 날짜 오름차순.

동기화 정책 — **`ok === true`인 매장에만 적용한다.** 그 매장의 행 중 이번 수집 결과에 리뷰번호가 없는 것은 삭제한다.

`ok !== true`인 매장의 행은 **읽지도 쓰지도 않는다.** 삭제도, 신규 추가도 하지 않고 그대로 둔다. 정상 수집된 매장이 하나도 없으면 파일을 열기만 하고 저장 없이 종료한다.

조회 기간 밖 과거 리뷰가 삭제되는 것은 의도된 동작이며, 이 엑셀은 "최근 30일 현황"이다. 과거 리뷰를 남겨야 하면 실행 전에 파일을 백업해 둘 것.

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
    saved = os.path.expanduser('~/skillwork/배민_저점수리뷰_backup.xlsx')
    wb.save(saved)
    print(f'[WARN] 원본 저장 실패 ({e}) → {saved}', file=sys.stderr)

print(json.dumps({'saved': True, 'new': len(new), 'deleted': deleted, 'rows': len(data),
                  'untouched_rows': untouched, 'synced': [s['storeName'] for s in ok_stores],
                  'skipped': [[s['storeName'], s.get('reason','')] for s in skipped],
                  'saved_to': saved, 'new_rows': new}, ensure_ascii=False))
ENDPY
```

- `saved: false` → 저장하지 않았다. `skipped` 사유를 그대로 사용자에게 알리고 재실행을 안내한다.
- 저장 성공 → `[OK] 엑셀 저장 (신규 N건, 삭제 N건, 총 N행)`. `skipped`가 비어 있지 않으면 **반드시 함께 보고한다.**
- `saved_to`가 backup 경로면 → 사용자에게 엑셀을 닫고 다시 실행해달라고 알린다.

---

## Step 6: 정리

```
tabs_close_mcp(tabId=<탭ID>)
```

---

## 최종 보고 형식

```
조회 기간: YYYY-MM-DD ~ YYYY-MM-DD (최근 30일)

| 매장 | 전체(N) | 수집 | 저점수 | 신규 | 별점분포(1~5) | 상태 |
|---|---|---|---|---|---|---|
| 김치찜의 정석 | 205건 | 205건 | 6건 | 1건 | 1/1/4/23/176 | 정상 |
| 퍽퍽살이 싫어 내가 만든 곱도리 | ... | | | | | ⚠️ 확인필요: <reason> |
| 참 제육 | ... |

엑셀: 신규 N건 추가, N건 삭제, 총 N행
```

- `전체(N)` 열은 `expectedTotal`을, `수집` 열은 `collected`를 그대로 쓴다. **둘을 합쳐 쓰지 않는다** — 두 값이 벌어지는 것 자체가 신호다. `expectedTotal`이 `null`이면 `확인필요`라고 적는다.
- `ok === false`인 매장은 상태 열에 `⚠️ 확인필요: <reason>`을 쓰고, **그 매장은 엑셀에 반영되지 않았음을 한 줄로 덧붙인다**("기존 행은 그대로 두었습니다"). 수집률 낮음도 여기 올라온다 — 로그에만 남기지 않는다.
- `parseFail > 0`이면 표 아래에 `별점을 읽지 못한 리뷰 N건 — [별점확인필요] 표시로 저장됨`을 덧붙인다.
- `authExpired`가 있으면 재로그인 안내를 맨 위에 올린다.
- 신규 저점수 리뷰가 있으면 그 내용을 매장별로 나열한다. 없으면 "신규 저점수 리뷰 없음"이라고만 쓴다. 단 `ok === false`인 매장이 있으면 "없음"이라고 단정하지 말고 **"확인 필요"**라고 쓴다.

---

## 트러블슈팅

| 증상 | 원인 | 대응 |
|---|---|---|
| 리뷰가 있는데 1건만 수집하고 끝남 | `전체(1,460)`처럼 쉼표가 든 숫자를 `\d+`로 읽어 1로 오인 | Step 3의 정규식은 `([\d,]+)` + 쉼표 제거. 절대 되돌리지 말 것 |
| 리뷰가 있는데 스크롤 없이 종료 | "전체(N)" 표기 변경으로 파싱 실패 | `expectedTotal`을 0이 아닌 `null`로 두고 무진전 감지로만 종료 |
| 별점이 44 같은 이상값 | SVG 색상 세기 fallback 오탐 | `aria-label` 우선 경로가 실패한 경우다. 카드 구조를 다시 확인할 것 |
| 리뷰번호는 맞는데 내용이 뒤섞임 | 카드 경계 오탐 | `ReviewContent-module` 경계 실패 → `no_card`/`merged` 경고 확인 |
| 필터 클릭이 먹힐지 않음 | 프로모션 팝업이 클릭을 가로채 | `_dismissAll()`이 선행되는지 확인 |
| 팝업 WARN이 뜨는데 실제 팝업은 없음 | 우측 하단 챗봇 위젯을 팝업으로 오탐 | `isChatbot` 제외가 살아있는지 확인 |
| `Failed to fetch (self-api.baemin.com)` | 크로스 오리진 차단 (정상) | API 경로를 되살리려 하지 말 것. 위 "설계 근거" 참고 |
| CDP 타임아웃 45초 | 배치 예산이 45초에 근접 | `_scroll` 예산을 25초 이하로 유지 |
| 스크롤이 정체되고 `hidden: true` | 탭이 백그라운드라 rAF 스로틀링 | 그때만 호출 사이에 스크린샷 1회 |
| 저장은 됐는데 파일이 계속 커짐 | 빈 행 누적 | `delete_rows`를 쓰는지 확인 (셀 None 비우기 금지) |
| "등록된 가게가 없어요" | 계정 불일치 | 해당 매장 계정으로 로그인 후 재시도 |
| 한 매장의 저점수가 통째로 사라짐 | 실패를 모르고 빈 `reviews`로 동기화함 | `ok` 플래그가 살아 있는지 확인. 저장 스크립트의 `ok_stores` 필터를 제거하지 말 것 |
| `총건수 확인 필요` (ok:false) | "전체(N)" 표기 형식 변경 | Step 3의 정규식 `전체\s*\(([\d,]+)\)` 확인. **`null`을 `0`으로 되돌리지 말 것** — 기존 엑셀 행이 지워진다 |
| `기간 필터 미확인` (ok:false) | 다이얼로그 구조 변경으로 필터 적용 실패 | 조회 범위를 모르는 채 동기화하면 안 된다. 필터를 고친 뒤 재실행 |
| `인증만료 — 수집 도중 세션 종료` | 스크롤 중 세션 만료 | 그 매장은 실패 처리되어 엑셀이 보존된다. 재로그인 후 그 매장만 재수집 |
| `수집률 낮음 N/M` | 스크롤이 끝까지 못 감 | 재실행. 반복되면 무진전 종료가 이른지, 게시중단 리뷰가 많은지 확인 |
| 게시중단된 리뷰가 결과에 안 보임 | 의도적 제외 (우리가 신고해 내린 리뷰) | 정상. 배민 "차단" 탭에서 직접 확인 |