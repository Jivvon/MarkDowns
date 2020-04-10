##### `실전코딩 HW3`  `201502119 정지원`

#### TDD 테스트 주도 개발

테스트를 작성한 후 실행하여 실패를 확인하고, 이후 성공하는 최소한의 코드를 만들면서 개발한다. 즉, **테스트 작성 후 코드 개발**

TDD 이유 : 통찰력을 유지하고 속도감있게 코드작업을 할 수 있다.
각각의 개별 테스트를 **Unit 테스트**라 한다.

##### FIRST

- First : 코드생성전 테스트 작성
- Isolated : 여러 환경과 별계로 독립적으로 실행
- Repeatable : 자동화되고 반복으로 실행 가능
- Self validating : 개발자의 의도대로 작동하는지 boolean으로 체크
- Timely : 적절한 시점에 작성이 되었는지 (Production Code 이전)

> **리팩토링**을 하는 최고의 방법은 **작고 쉽게 언제든 되돌릴 수 있는 스텝**을 밟는 것이다.

##### red-green-refactor 레드 그린 리팩토링

1. red : 테스트 실패
2. green : 테스트 성공
3. refactor : 리팩토링
4. 커밋.

**integration 통합** : 변경 내용이 적을수록 부담이 덜하다. TDD라면 조금이라도 변경 후 integration.

#### junit

프로그래머 주도 테스트 프레임워크 for **JAVA**

Annotation (주석) : 소스코드의 메타 데이터를 가지고 있음. 즉, 자신의 정보

- @BeforeClass : 클래스 전 한번 실행.
- @Before : 테스트 전에 실행
- @test `필수` : 테스트를 하는 코드
- @After : 테스트 끝난 후 실행
- @AfterClass : 클래스 끝난 후 한번 실행.
- @Ignore : 테스트를 실행하지 않고 넘어간다. (ignore했을 때, 무시되어 테스트가 없으면 나머지 이노테이션 실행도 되지 않는다.

##### assert문 (self validating)

- assertTrue(조건) / assertFalse(조건) : 참이면 실행 / 거짓이면 실행
- assertArrayEquals([1,2], [1,2]) : 실제값과 기댓값 검사
- assertNull(값) / assertNotNull(값) : null이면 실행 / null이 아니면 실행
- assertNotSame(값) : 비교값이 다르면 실행
- fail : 무조건 실패시킨다.

> 좋은 개발은 문제를 발견하면 바로 개선하는 것이다. 
> @Test에서 중복되는 코드는 @Before에 추가하는 리팩토링 과정을 거쳐야 한다.
> 리팩토링 시, 기존의 코드는 두고 새로운 코드를 만들어 완성되면 기존의 코드를 대체하는 방법을 ‘병렬변경’이라고 한다.