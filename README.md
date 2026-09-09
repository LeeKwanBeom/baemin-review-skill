# baemin-review-skill

배달의민족 셀프서비스에서 저점수(1~3점) 리뷰를 수집해 엑셀에 저장하는 Claude 스킬.

## 이 저장소의 역할

**기록 보관소다. 실행 경로가 아니다.**
스킬은 설치본을 읽고 돈다 — 그 경로는 스킬 호출 헤더의 `Base directory for this skill:` 줄이 알려준다
(2026-09-09 실측: `/root/.claude/skills/synced/<uuid>_<uuid>/baemin-review/SKILL.md`. 예전에 적혀 있던 `/mnt/skills/plugins/...` 는 이 환경에 없다).
이 저장소는 그 사본과 점검 기록을 보관한다.

| 파일 | 무엇 |
|---|---|
| `SKILL.md` | 설치본 사본이자 정본. 수정은 여기서 하고 `.skill` 로 패키징한다 |
| `audit/checklist.md` | 정기 점검 절차 |
| `audit/last-audit.md` | 회차별 점검 기록 |

## 수정 흐름

저장소 `SKILL.md` 수정 → push → `.skill` 재패키징 → Claude 설정에서 재업로드.
**재업로드 전에는 실행에 반영되지 않는다.**

## 정기 점검

```
/baemin-review
정기 점검. 아직 고치지 말고 진단만. 뭘 고칠지는 내가 고를게.
저장소를 clone해서 audit/checklist.md 를 읽고 그대로 수행해라.
git clone https://github.com/LeeKwanBeom/baemin-review-skill
push 토큰: (여기에 붙여넣기)
```
