# Implementer Skill

역할: Implementer. TDD Red-Green-Refactor 사이클로 구현하고 검증한다.

## 언제 이 스킬을 사용하나요?

- `/task` 워크플로우 Step 3 실행 시 (`APPROVED: <run-id>` 이후)
- 승인된 계획을 TDD 방식으로 구현할 때

---

## TDD 사이클

각 기능을 다음 순서로 반복한다. **한 사이클이 끝난 뒤 다음 케이스로 넘어간다.**

```
[Red]      실패하는 테스트 작성
[Green]    테스트를 통과하는 최소한의 코드 구현
[Refactor] 동작을 바꾸지 않고 코드 개선
   ↓
다음 케이스로 반복
```

---

## Red: 실패하는 테스트 작성

### AAA 패턴 (Arrange - Act - Assert)

```java
@Test
void 중복_이메일로_회원가입하면_예외가_발생한다() {
    // Arrange
    given(userRepository.existsByEmail("dup@example.com")).willReturn(true);

    // Act & Assert
    assertThatThrownBy(() -> userService.signup(new SignupRequest("dup@example.com", "pw")))
            .isInstanceOf(IllegalArgumentException.class);
}
```

### 테스트 네이밍

"무엇을 하면 어떻게 된다" 형식으로 행동을 서술한다. 각 테스트 메서드명은 한글로 작성하고, DisplayName을 반드시 붙여 작성한다.

```java
// 좋은 예
@DisplayName("이메일 회원가입")
void 새_이메일로_회원가입하면_저장된다();
@DisplayName("중복 이메일 회원가입시 예외 발생")
void 중복_이메일로_회원가입하면_예외가_발생한다();

// 나쁜 예
void test1();
void signupTest();
```

### 테스트 종류 (Spring Boot)

**단위 테스트** — 서비스/도메인 로직, Mockito로 의존성 격리

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock UserRepository userRepository;
    @InjectMocks UserService userService;
}
```

**통합 테스트** — API 엔드포인트, 실제 컨텍스트로 HTTP 검증

```java
@SpringBootTest
@AutoConfigureMockMvc
@Transactional
class UserControllerTest {
    @Autowired MockMvc mockMvc;
}
```

---

## Green: 최소한의 구현

- 테스트를 통과시키는 **가장 단순한 코드**만 작성한다
- 과도한 추상화, 미래를 위한 설계 금지
- 테스트가 Green이 될 때까지 구현에만 집중한다

---

## Refactor: 코드 개선

테스트가 통과한 뒤 다음을 점검한다:

- 중복 제거
- 네이밍 명확화
- 불필요한 복잡도 제거

**규칙: 리팩터링 중 테스트가 깨지면 즉시 되돌린다.**

---

## 케이스 처리 순서

승인된 계획의 테스트 전략을 기준으로 다음 순서로 처리한다:

1. 정상 케이스 → Red-Green-Refactor
2. 예외 케이스 → Red-Green-Refactor
3. 경계값 케이스 → Red-Green-Refactor

---

## 최종 검증

모든 케이스 구현 완료 후:

1. **전체 테스트 실행** — 모든 테스트 통과 확인
2. **빌드** — 빌드 성공 확인
3. **정적 검증** — 린트, 타입 검사 등 (해당하는 경우)

---

## Reviewer 또는 QA 피드백 수신 시

오케스트레이터가 수정 지침과 함께 재실행을 요청한 경우:

- 전체 재구현 금지 — 수정 지침에 해당하는 범위만 수정한다
- 수정 후 전체 테스트를 재실행하여 기존 테스트가 깨지지 않는지 확인한다

---

## 계획 외 변경이 필요한 경우

즉시 중단하고 기록한다:

```
CHANGE_REQUEST: [이유와 내용]
```

재승인 없이 계속 진행하지 않는다.

---

## 출력 형식

```
[구현 결과]
- 사이클별 진행 내용 (케이스명: Red → Green → Refactor)
- 생성/수정한 파일 목록

[검증 결과]
- 빌드: 성공 / 실패 (원인)
- 테스트: N개 통과 / M개 실패 (실패 목록)
```
