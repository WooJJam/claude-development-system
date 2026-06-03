# AI 개발 워크플로우

이 프로젝트는 **Superpowers** 기반의 AI 개발 워크플로우를 따른다.

---

## 워크플로우 적용 조건

**`/task` 커맨드로 명시적으로 요청한 경우에만** 아래 워크플로우를 적용한다.
그 외 일반 요청은 워크플로우 없이 바로 처리한다.

---

## 워크플로우 순서 (`/task` 실행 시)

```
1. /brainstorming              요구사항 정리 + 코드베이스 조사 (Researcher 역할)
2. /writing-plans              구현 계획 수립 → 승인 대기
   ↓ APPROVED: <run-id>
3. /test-driven-development    실패하는 테스트 작성 (Test Engineer 역할)
4. /executing-plans            구현 + 검증 (Implementer / Verifier 역할)
5. /requesting-code-review     코드 리뷰 + QA (Reviewer / QA Tester 역할)
6. /finishing-a-development-branch  완료 기준 확인 + 최종 요약
```

---

## 핵심 규칙 (`/task` 실행 중에만 적용)

- `APPROVED: <run-id>` 없이 코드 수정 금지
- 계획 외 파일 변경 시 즉시 중단 → `CHANGE_REQUEST:` 기록 → 재승인
- 승인 형식: `APPROVED: ai-run-YYYY-MM-DD-NNN`
- "ok", "진행해" 등 모호한 표현은 승인으로 인정하지 않는다

---

## 완료 기준

- [ ] 승인된 계획 기준으로 구현
- [ ] 검증 명령 결과 기록
- [ ] 코드 리뷰 결과 기록
- [ ] QA 결과 기록
- [ ] 최종 변경 요약 작성
