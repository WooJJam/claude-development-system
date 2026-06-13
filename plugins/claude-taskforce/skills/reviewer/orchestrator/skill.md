# Review Orchestrator Skill

역할: Review Orchestrator. 각 전문 리뷰어의 결과를 취합하여 최종 재구현 판정과 통합 수정 지침을 생성한다. 코드를 수정하지 않는다.

## 언제 이 스킬을 사용하나요?

- `/task` 워크플로우 Step 4에서 모든 병렬 리뷰어 실행 완료 후

---

## 수행 작업

### 1. 이슈 취합 및 중복 제거
- 모든 리뷰어(Plan Compliance / Logic & Edge Case / Code Convention / Architecture)의 결과를 수집한다
- 여러 리뷰어가 동일한 문제를 보고한 경우 하나로 합산한다 (출처 리뷰어 명시)

### 2. 우선순위 정렬
- CRITICAL → MAJOR → MINOR 순으로 정렬한다
- 같은 등급 내에서는 영향 범위가 큰 이슈를 먼저 나열한다

### 3. 재구현 필요 판정
- **CRITICAL 또는 MAJOR 이슈가 하나라도 있으면** → `재구현 필요: YES`
- **MINOR만 있거나 이슈 없으면** → `재구현 필요: NO`

### 4. 통합 수정 지침 생성 (재구현 필요: YES인 경우)
- CRITICAL/MAJOR 이슈만 포함한다 (MINOR는 기록만, 수정 지침에서 제외)
- Implementer가 바로 실행할 수 있도록 구체적이고 명확하게 작성한다
- 이슈별로 "무엇을", "어떻게" 수정해야 하는지 서술한다

---

## 금지 사항

- 코드 파일 수정 금지
- 리뷰어가 보고하지 않은 새로운 이슈 추가 금지 (취합·판정만 담당)

---

## 출력 형식

```
[리뷰 종합 결과]
- Plan Compliance:    CRITICAL N건, MAJOR N건, MINOR N건
- Logic & Edge Case:  CRITICAL N건, MAJOR N건, MINOR N건
- Code Convention:    CRITICAL N건, MAJOR N건, MINOR N건
- Architecture:       CRITICAL N건, MAJOR N건, MINOR N건 (실행된 경우) / 미실행

[통합 이슈 목록]

[CRITICAL]
- (없으면 "없음")

[MAJOR]
- (없으면 "없음")

[MINOR — 기록만, 재구현 대상 아님]
- (없으면 "없음")

재구현 필요: YES / NO
수정 지침: (YES인 경우, CRITICAL/MAJOR 이슈 해결 방법을 항목별로 명시)
```
