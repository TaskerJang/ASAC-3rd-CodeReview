## @Controller 에서 클라이언트로부터 파라미터를 받는 방법 4개

- **@PathVariable** : URI 상의 가변(변수)
- **@RequestParam** : 단일 파라미터
    - **@ModelAttribute** : 다수 파라미터 (DTO 객체로 정의 필요@Setter)
- **@RequestBody** : POST Body(JSON, XML 등)

## **Java 8 Functional Interface 함수형 인터페이스**

### 익명 클래스와 익명 구현 객체

- 객체지향 프로그래밍 Java 에 함수형 프로그래밍 패러다임 도입을 위해 (람다 함수) 추가된 개념
    - 기존에 제공되었던 **익명 클래스** 와 같이, 이를 위해 **익명 구현 객체** 를 제공한다.

      > 잠시만, 왜 “익명” 클래스니 함수이니 이런것들이 필요한가? = **“일회용” 클래스** or **함수**
      >
      > - **한번만 쓰고 버릴것** = 일회용 종이컵과 같은것
      > - 한번만 쓰고 버릴것인데, 메모리에 상주시키거나 따로 관리하기 싫은 경우
        - **익명 클래스 : 클래스를 상속한**(extends) **‘익명’ 클래스**
            - 상속할 **부모 클래스**가 존재해야한다는 단점

            ```java
            class Animal {
                public String bark() {
                    return "엉엉";
                }
            }
            ```

            - 나는 위 부모 클래스를 상속받은 Dog 자식 ~~개자식~~ 클래스를 만들고싶어
                - **방법 1) 자식 클래스**를 정의한 **다음** 인스턴스화

                    ```java
                    class Dog **extends Animal** {
                        @Override
                        public String bark() {
                            return "멍멍";
                        }
                    }
                    Animal dog = new Dog(); // **다음** 인스턴스화
                    dog.bark();
                    ```

                - **방법 2) 익명 클래스**를 정의와 **동시에** 인스턴스화 ⇒ **이번 주제**

                    ```java
                    Animal dog = new Animal() { // **동시** 인스턴스화
                        @Override
                        public String bark() {
                            return "멍멍";
                        }
                    };
                    dog.bark();
                    ```

        - **익명 구현 객체 : 인터페이스를 구현한**(implements) **‘익명’ 구현 객체**
            - 클래스 없이 바로 **인터페이스**를 통해서만 구현

            ```java
            interface IAnimal {
                public String bark();
            }
            ```

            - 나는 인터페이스 기반으로 Dog 자식 ~~개자식~~ 클래스를 만들고싶어
                - **방법 1) 구현 클래스**를 정의한 **다음** 인스턴스화

                    ```java
                    class Dog **implements IAnimal** {
                        @Override
                        public String bark() {
                            return "멍멍";
                        }
                    }
                    Animal dog = new Dog(); // **다음** 인스턴스화
                    dog.bark();
                    ```

                - **방법 2) 구현 클래스**를 정의와 **동시에** 인스턴스화

                    ```java
                    IAnimal dog = new IAnimal() { // **동시** 인스턴스화
                        @Override
                        public String bark() {
                            return "멍멍";
                        }
                    };
                    ```


### 익명 구현 객체를 통한 익명 함수 정의 (Java 에서)

다시 한번 생각해보기. 왜? Java 는 **익명 구현 객체** 문법을 만들었을까?

- **함수형 프로그래밍** = **일급함수** + **순수함수**
    - **일급함수** = 함수가 **변수, 파라미터, 반환**으로 사용될 수 있다.
    - **순수함수** = No Side-Effects **참조 투명성 = 반복 호출해도 언제나 같은 결과(or 상태)**
- Java 은 **함수**가 아닌 **메서드**. 즉, 우리가 말하는 함수는 **메소드로써 꼭 객체 내 정의되어야한다**
    - 그래서, Java 에서 **익명 함수**를 정의하기 위해서는 **익명 구현 객체**가 꼭 필요하다.
- 인터페이스를 통한 **익명 구현 객체** 안에 **익명 함수**를 정의하여 사용 ⇒ **아래의 예시를 통해 알아봅시다**
    - **예시)** 만약에 users 라는 Array 가 있고, 그 Array 를 특정 값 기준으로 나열하고싶다면

    ```java
    User[] users = {
        new User("Aaron", 12),
        new User("Baron", 11),
        new User("Caron", 13),
        new User("Daron", 10),
    };
    ```

    - **User** 에서 **나이값**을 기준으로 **오름차순**으로 나열하고싶다면
        - 어떤거 기준으로 나열할지에 대한 **함수 정의(익명 함수)**를 **Sort** 함수의 **Parameter** 로 넘겨야한다

    ```java
    // - 익명 함수와 익명 구현 객체에 대한 짧은 스토리텔링
    // 1) 익명 함수를 파라미터로 받아야합니다.
    Arrays.sort(users, **익명 함수(어떤걸 기준으로 나열할지)**)
    // 2) 우리가 익히 아는 방식은 아래와 같이 익명 함수를 Lambda 라 부르며 주입시켜주죠.
    Arrays.sort(users, **(피비교대상1, 피비교대상2) -> { ... }**)
    // 3) 아까 설명했듯 Java 에서 익명 함수를 쓰기 위해서는 익명 구현 객체를 쓴다고 말씀드렸습니다.
    Arrays.sort(users, **new Comparator<User>() {
        @Override
        public int compare(User u1, User u2) {
            return Integer.compare(u1.numb, u2.numb);
        }
    }**)
    // 4) Java 의 익명 구현 객체는 다음과 같이 최종적으로는 Lambda 형태로 쓸 수 있습니다. :)
    Arrays.sort(users, **(u1, u2) -> Integer.compare(u1.numb, u2.numb)**);
    // +) 여담이지만, 사람들이 이걸 되게 많이 사용하기 때문에 다음과 같이 편한걸 제공해주네요
    Arrays.sort(users, **Comparator.comparingInt(u -> u.numb)**);
    ```


### 저기요, 지금까지 Functional Interface 함수형 인터페이스란말 한마디도 안나왔어요.

익명 구현 객체는 우리가 어떤 익명 함수를 정의할지에 따라 종류가 다음과 같이 나뉠수 있습니다.

**익명 함수가 어떤 형태가 될 수 있을까요?** 단순하게 쉬운것부터 생각해보면 아래와 같습니다.

- 파라미터가 없고, 결과값이 **있는** 경우 : (  ) ⇒ (**R**) = **Supplier** 익명 구현 객체
- 파라미터가 **있고**, 결과값이 없는 경우 : (**T**) ⇒ (  ) = **Consumer** 익명 구현 객체

### Functional Interface

**익명 함수가 될 수 있는 형태들을 미리 정의하여 제공** = **익명 구현 객체의 Syntatic Sugar**

- **파라미터 0~1개**
    - **Function** : **(T) → (R)** | 익명 함수명 apply
        - **Predicate** : **(T) → (BOOLEAN)** | 익명 함수명 test
        - **UnaryOperator** : **(T) → (T)** | 익명 함수명 apply
    - **Supplier** : **(  ) → (R)** | 익명 함수명 get
    - **Consumer** : **(T) → (  )** | 익명 함수명 accept
- **파라미터 2개**
    - **BiFunction** : **(T, U) → (R)** | 익명 함수명 apply
        - **BiPredicate** : **(T, U) → (BOOLEAN)** | 익명 함수명 test
        - **BiUnaryOperator** : **(T, T) → (T)** | 익명 함수명 apply
    - ~~Supplier : **(  ) → (R)** | 익명 함수명 get~~ ⇒ **파라미터가 없는 경우라서 제외**
    - **BiConsumer** : **(T, U) → (  )** | 익명 함수명 accept
- 파라미터별 함수 역할에 따라 색깔로 나누어봤었는데, 그림 참조

 ![IMG_F0D86A7440EE-1](https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/56432947-631a-40c2-951a-326ef6e7f356)



## Java 추상 클래스(Abstract Class) 와 인터페이스(Interface) 차이

객체지향 프로그래밍의 4가지 요소(추상화, 다형성, 캡슐화, 상속)를 위해 제공되는것 **Interface 인터페이스**

- **인터페이스**
    - **형태**만을 정의
        - **형태** : 어떤 메소드를 가져야하는지 명시

    ```java
    public interface KafkaListener<T> {
        KakfaListnerType getType();
        void consume(T consumed);
    }
    ```

    - 위 예시에서 사용한 KakfaListnerType 은 Enum 으로써 아래와 같이 정의된다.
        - **Lombok 미활용시 Enum 정의** (처음 배울때는 이걸로 배우는게 Lombok 이해에도 좋습니다.)

            ```java
            enum MessageType {
                EMAIL,
                SMS;
            }
            
            enum KakfaListenerType {
            		USER_KAFKA("this is user", MessageType.EMAIL),
                PAYMENT_KAFKA("this is payment", MessageType.SMS);
            
                private final String message;
                private final MessageType type;
            
                KakfaListenerType(String message, MessageType type) {
                    this.message = message;
                    this.type = type;
                }
            
            		public String getMessage() {
                    return message;
                }
            
                public MessageType getType() {
                    return type;
                }
            }
            ```

        - **Lombok 활용시 Enum 정의**

            ```java
            enum MessageType {
                EMAIL,
                SMS;
            }
            
            @Getter
            @AllArgsConstructor
            enum KakfaListenerType {
            		USER_KAFKA("this is user", MessageType.EMAIL),
                PAYMENT_KAFKA("this is payment", MessageType.SMS);
            
                private final String message;
                private final MessageType type;
            }
            ```

- **추상클래스** : 클래스와 인터페이스 중간 성격
    - **형태**와 **구현**을 혼합 정의
        - **형태** : 어떤 메소드를 가져야하는지 명시
        - **구현** : 구현 메소드를 정의
    - 아까 살펴본 **인터페이스** 예시를 이어받아 **추상클래스** 정의

    ```java
    @Slf4j
    public abstract class KafkaAbstractListener<T> implements KafkaListener<T> {
    
        @PostConstruct
        public void instantiated() {
    			  log.info("[Abstract Class] {} is created", this.getType().name());
    		}
    
    		@PreDestroy
        abstract void destroy();
    }
    ```

    - 인터페이스와 추상클래스의 **차이점**은 implements 로 구현되냐? extends 로 확장되나?
        - 앞서 살펴본 **인터페이스**는 **implements** 로 구현되고
        - 지금 배우는 **추상클래스**는 **extends** 로 확장된다.
    - 인터페이스와 추상클래스의 **공통점**은 **미구현 함수**(**추상메소드** 라고 부름) ****를 둘 수 있다.
        - **인터페이스**에도 **미구현 함수**(**추상메소드**)가 있고

            ```java
            public **interface** KafkaListener<T> {
            		...
                **void consume(T consumed);**
            }
            ```

        - **추상클래스**에도 **미구현 함수**(**추상메소드**)가 있다.

            ```java
            public **abstract class** KafkaAbstractListener<T> implements KafkaListener<T> {
            		...
                **abstract void destroy();**
            }
            ```


### 추상 클래스(Abstract Class) 와 인터페이스(Interface) 현업에서의 구현 예시

코드의 부분만을 따왔기에, 실제 구동은 안될것임. 보고 이런식으로 쓰는구나 이해만 하도록

- **인터페이스**

    ```java
    public interface TravelKafkaListener<T> {
    	void consume(
    		@Payload T message,
    		@Header(KafkaHeaders.RECEIVED_PARTITION_ID) int partition,
    		@Header(KafkaHeaders.RECEIVED_TOPIC) String topic,
    		@Header(KafkaHeaders.OFFSET) String offset
    	)
    }
    ```

- **추상클래스**

    ```java
    @Slf4j
    public abstract class AbstractKafkaListener<T, U> implements KafkaListener<T> {
    	@PostConstruct
    	private void registered() {
    		log.info("[KAFKA-LISTENER] {}", this.getClass().getName());
    	}
    
    	@Getter
    	@Setter
    	private BiConsumer<T, U> completeCallback;
    
    	protected void consumeCompleteCallback(T message, U result) {
    		log.info("[KAFKA-LISTENER] {} resultDto: {}", this.getClass().getName(), message);
    		if (ObjectUtils.isEmpty(completeCallback)) {
    			return;
    		}
    		completeCallback.accept(message, result);
    	}
    }
    ```

- **추상클래스 (+ 인터페이스) 구현체**

    ```java
    
    @RequriedArgConstructor
    public class KafkaMemberListener extends AbtractKafkaListner<MemberStatusDto, String> {
    	private final MemberApiCacheService memberApiCacheService;
    	private final MemberService memberService;
    
    	@Override
    	@KafkaListener(topics = "${kafka.topic.member}", containerFactory = "memberKafkaListenerContainerFactory")
    	public void consume(
    		@Payload MemberStatusDto message,
    		@Header(KafkaHeaders.RECEIVED_PARTITION_ID) int partition,
    		@Header(KafkaHeaders.RECEIVED_TOPIC) String topic,
    		@Header(KafkaHeaders.OFFSET) String offset
    	) {
    		long memberId = message.getMemberSrl();
    		log.info("[KAFKA-LISTNER] update member status, memberId: {}", memberId);
    		memberService.sync(memberId);
    		memberApiCacheService.evictCachedMemberDtoByMemberId(memberId);
    	}
    }
    ```


### + Java 정적 메소드

저번에 객체 생성 방법에 대해 설명하다가 정적 메소드 언급을 잠깐했었는데, **인스턴스 없이 사용 가능한 함수**

자바스크립트에서도 정적 메소드가 존재하는데, `NumberUtil.calculate()` 이런 형태로 사용

- 인스턴스 생성 없이 호출이 가능하여, 유틸리티 관련 함수를 만드는데 유용하게 사용된다.
    - 자바 예시

        ```java
        public final class CommonUtils {
            
            public static LocalDateTime stringToLocalDateTime(String strDate) {
                return LocalDateTime.parse(strDate, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
            }
         
            public static String localDateTimeToString(LocalDateTime localDateTime) {
                return localDateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
            }
         
            public static boolean isEmailValid(String email) {
                return Patterns.EMAIL_ADDRESS.matcher(email).matches();
            }
        }
        ```

    - 자바스크립트 예시

        ```java
        class Article {
          constructor(title, date) {
            this.title = title;
            this.date = date;
          }
        
          static createTodays() {
            // this는 Article입니다.
            return new this("Today's digest", new Date());
          }
        }
        ```

- 추가적으로 **정적 변수**도 존재

## Lombok 어노테이션

Spring 을 사용함에 있어서, 크게 2개 타입의 Annotation 을 접할것이다.

- **Spring 에서 제공하는 Annotation** (예, @Controller, @Repository 등)
    - Layered Architecture 에 맞춘 Component 나 옵션 설정 시 사용
        - 예) **@Controller** : 컨트롤러 정의를 위한 Annotation
        - 예) **@RequestMapping** : URL + METHOD 와 함수 연결

 <img width="990" alt="Untitled (2)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/7d5a6563-47ef-43e4-b19b-137d16400861">


- **Lombok 에서 제공하는 Annotation** (예, @Setter, @Getter 등)
    - 예) **@Setter** : 해당 클래스 내 모든 필드 (혹은 특정 필드) 에 Setter 함수 생성
    - 예) **@Getter** : 해당 클래스 내 모든 필드 (혹은 특정 필드) 에 Getter 함수 생성

  <img width="700" alt="Untitled (3)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/a14344ba-383d-4d64-b8b6-0c730855a3ad">


- **Lombok 원리** : 컴파일 시점에 Annnotation Processor 를 사용하여 Abstract Syntax Tree 조작
    - **Java (.java → .class) 컴파일 과정**
        - 문법(구문)을 분류하여 AST : Abstract Syntax Tree 생성
        - AST : Abstract Syntax Tree 기반으로 Bytecodes 로 모두 변환 완료
            - Lombok 은 이 AST : Abstract Syntax Tree 에 원하는 함수 주입
            - 이렇게 Lombok 에 의해 수정된 AST 는 최종 Bytecodes 로 생성된다.

### 자주 사용하는 Lombok 어노테이션

- **@Getter** : 해당 필드에 대해 get() 메서드 생성
- **@Setter** : 해당 필드에 대해 set() 메서드 생성
- **@ToString** : 해당 객체에 대해 toString() 메서드 생성
    - 따로 toString() @Override 로 상세 구현하지 않아도 될 정도로 깔끔하게 출력해준다.
    - exclude 를 통해 출력하지 않을 필드들을 설정할 수 있다.
    - **예시 코드 및 toString() 출력 결과**

        ```java
        @AllArgsConstructor
        @ToString(exclude = "id")
        public class Member {
          private Integer id;
          private String name;
          private String email;
        }
        ```

        - 자동 생성된 toString() 출력 결과

        ```java
        Member member = new Member(1, "test", "test@naver.com");
        System.out.println(member);
        // Output : Member(name=test, email=test@naver.com)
        ```

- **@AllArgsConstructor** : 해당 객체의 생성자 생성 (**모든** 인자)
- **@RequiredArgsConstructor** : 해당 객체의 생성자 생성 (**필수** 인자)
- **@NoArgsConstructor** : 해당 객체의 생성자 생성 (**인자없는** 생성자)
    - **예시 코드 : @All / @Required / @No 3개 예시 모두**

        ```java
        @NoArgsConstructor
        @AllArgsConstructor
        @RequiredArgsConstructor
        public class Member {
          @NonNull
          private Integer id;
          @NonNull
          private String name;
          private String email;
        }
        ```

        - **호출 가능**

        ```java
        // NoArgsConstructor
        Member member1 = new Member();
        
        // AllArgsConstructor
        Member member2 = new Member(1, "A");
        
        // RequiredArgsConstructor
        Member member3 = new Member(2, "B", "B@naver.com");
        ```

- @****EqualsAndHashCode**** : Java Bean (Spring Bean 아님) 이 갖추어야할 `equals`, `hashCode` 생성
    - **예시 코드**

        ```java
        public class User {
          private Integer Id;
        }
        
        @EqualsAndHashCode(callSuper = true)
        public class Member extends User {
          private String name;
          private String email;
        }
        ```

        - 객체들을 비교할때, 생성된 `equals` 함수를 통해 비교 (내부적으론 `hashCode` 함수 사용)

            ```java
            Member member1 = new Member();
            member1.setId(1);
            member1.setName("A");
            member1.setEmail("A@naver.com");
            
            Member member2 = new Member();
            member2.setId(2);
            member2.setName("A");
            member2.setEmail("A@naver.com");
            ```

            - @EqualsAndHashCode(callSuper = **true**) 일때, ID 가 다르면 다른것

                ```java
                member1.equals(member2); // **false**
                ```

            - @EqualsAndHashCode(callSuper = **false**) 일때, ID 가 달라도 이외 값들이 같으면 같은것

                ```java
                member1.equals(member2); // **true**
                ```

- **DTO = @Data** : 위 모든 어노테이션을 한번에 모아 정의해놓은것

  > **DTO** 란 ? **가변** 객체 ⇒ 데이터를 불러다 쓸수도 있고, 객체 내 데이터를 조작할수도 있고
  >
    - **사용에 주의해야하는 이유**
        - **이유 1)** 일일히 필요 어노테이션 작성하기 귀찮다는 이유로 사용하지 않을 함수를 노출하게된다.
            - 예) VO 로 쓰기 위해 어떤 상황에서도 값이 바뀌거나 설정되면 안되는데, @Setter 포함
        - **이유 2)** 각 어노테이션별 세부 설정이 불가능
            - 예) @ToString 에 exclude 옵션을 넣을 수 없음
- **VO = @Value** : @Data 에서 @Setter 만 제외한 것

  > **VO** 란 ? **불변** 객체 ⇒ 데이터를 불러다 쓸수만 있음 (조작 불가)
  >
    - **위 @Data 사용에 주의해야하는 이유 중 이유 1) 에 대한 대안이라고 생각해도 된다.**

### 객체 생성하는 방법 3가지

1. **생성자**
    - **@NoArgsConstructor + @Setter**
        - 먼저 빈 객체를 생성한뒤, 필요에 따라 원하는 필드값을 주입하는 방식
    - **Custom Constructor 만들기**
        - 필요한 필드값만 주입받는 생성자
    - **@AllArgsConstructor**
        - 모든 필드값을 다 주입해야하는 생성자
2. **빌더**
    - **장점** : 객체 생성을 위해 내가 원하는 필드만 설정할 수 있고, 순서도 뒤죽박죽으로 할 수 있다.
        - 생성자와 달리 필드값 주입 순서에 구애받지않는다.

        ```java
        RequestDto dto = RequestDto.builder()
                        .hello("A")
                        .world("B")
        								.build();
        ```

        ```java
        RequestDto dto = RequestDto.builder()
                        .world("B")
                        .hello("A")
        								.build();
        ```

        - 객체 생성을 위한 필드값 주입 역할을 마음껏 분리할 수 있다.

        ```java
        RequestDto.RequestDtoBuilder isBuildingSetHello =
        				RequestDto.builder()
                .hello("A");
        ```

        ```java
        RequestDto.RequestDtoBuilder isBuildingSetWorld =
        				isBuildingSetHello
                .world("B");
        ```

        ```java
        RequestDto initialized = isBuildingSetWorld.build();
        ```

    - **단점 :** 객체 생성시 생성되지 말아야하는 필드까지 빌더로 설정할 수 있다.
        - 객체 생성을 위해 Builder 에 들어가지 말아야하는 필드에 제약같은것을 둘 수 없다.
    - **예시 코드 @Builder 와 함께 Custom Builder 사용**

        ```java
        @Getter
        @Builder
        public class RequestDto {
            private String hello;
            private String world;
        
            public static class RequestDtoBuilder {
                public RequestDtoBuilder hello(String value) {
        						// 요구사항 : hello 필드는 "BYE" 를 할당하려하면 에러를 발생시켜야한다.
                    if (value.equals("BYE")) {
                        throw new RuntimeException("Unacceptable Word");
                    }
                    this.hello = value;
                    return this;
                }
            }
        }
        ```

3. **정적 팩토리 메소드**
    - 객체를 생성할 수 있는 방법이 오직 하나의 정적 메소드 방식으로만 가능하도록 설정
        - **팩토리 메소드** = 객체를 생성 혹은 반환하는것
    - **예시 코드** : 정적 메소드의 의의는 **기존 객체에서 새로운 객체로의 변환**

      > 아래와 같이 생성자를 Lombok 어노테이션으로 Private 형태로 정의/감추어도 된다.
      >
        - @NoArgsConstructor(**access = AccessLevel.PROTECTED**)
        - @AllArgsConstructor(**access = AccessLevel.PROTECTED**)

        ```java
        @Getter
        class ParsedRequestDto {
            private final String hello;
            private final String world;
        
            private ParsedRequestDto(String hello, String world) {
                this.hello = hello;
                this.world = world;
            }
        
            public static ParsedRequestDto of(RequestDto requestDto) {
                String calculated = Calculator.caculate(
                    requestDto.getHello(),
                    requestDto.getWorld()
                );
                ParsedRequestDto created = new ParsedRequestDto(
                    requestDto.getHello(),
                    requestDto.getWorld()
                );
                return created;
            }
        }
        ```


**11월 02일 (목) - 보충 수업 내용**

## Spring @Controller 잘 작성하는 방법

### **Exception 발생 근원지(?, Source)**

Enum Exception 처리 로직을 String → Enum 로직과 떨어트리는게 좋은가? **No**

- Enum 내 String → Enum 변환 **정적 메서드**
    - **이전** : null 을 반환하고, `from()` 메서드 **외부에서 Exception 발생**

      <img width="1000" alt="Untitled (4)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/7884c8f0-bc4f-4d31-8dfe-8fc3a3dfc010">


    - **이후** : **String** 에 해당하는 **Enum** 이 존재하지 않는 경우 **내부에서 Exception 발생**
        - Enum 미존재 Exception 은 String → Enum 변환 로직과 분리될 수 없다.

      <img width="1000" alt="Untitled (5)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/aac6a52f-4ea0-41a0-8f50-9e6fea63aeca">



### @RequestBody 의 Null + Validation 처리

GetProducWithCondition 메서드 내 SearchReqDto 내 필드를 가져다 쓰는데 2가지를 선처리한다.

- **Null 인지 여부를 검사**하고, 심지어 기본값을 주입해주는 로직을 가지고있다.
- **유효한 날짜인지 여부를 검사**한다.
    - **이전** : 이 모든 로직들이 아래 코드에서 **약 16줄의 라인을 차지한다.**

     <img width="1000" alt="Untitled (6)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/27216b69-3908-4e19-a63a-bf039ac1b0c0">


    - **이후** : 이 모든 로직들은 **DTO 내 Getter 메서드 호출할때 혹은 JSON → 객체 Deserialize 때로 이관**
        - DTO 는 다음과 같다. 이를 기준으로 아래 순서에 따라 리팩토링 시도

         <img width="480" alt="Untitled (7)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/b9f580eb-a9d5-49a8-8cea-4b380a432b64">


        1. **Null 을 입력하지 않도록 막는 책임을 백엔드가 가져갈것인가, 프론트엔드가 가져갈것인가?**
            - **프론트엔드가 가져간다면**, 백엔드는 **@NotNull 로 절대 Null 이어서는 안된다는 조건을 추가**
            - **백엔드가 가져간다면**, 백엔드는 해당 필드를 Nullable 로 판단하고, **기본값을 넣어주도록 설정**
                - 기본값을 설정할때도 sel_date 와 sel_date2 처럼 필드간 의존성을 두는것보다

                  예) sel_date = “2023-10-10” & sel_date2 = “2023-10-11”

                - sel_date 와 Integer stay 와 같이 필드간 의존성을 없애는것이 좋지 않을까?

                  예) sel_date = “2023-10-10” & stay = 1

        2. **Validation 여부 판단을 Dto 를 사용하는곳에서 별개의 로직으로 구현하지않는것이 어떨지?**
            - 어떤 시점에 Validation 판단할것인가? 2가지 정도로 나열해 볼 수 있지 않을까?
                1. **@Valid** → JSON 을 @RequestBody 객체로 Deserialize 할때 (Jackson 에서)
                    - 참고 코드 : **[DTO 내 특정 필드에 대해 커스텀 어노테이션을 통해 필터링](https://stackoverflow.com/a/41718607)**할거면

                      아래와 같이 @Valid 를 꼭 넣어주어야한다.

                     <img width="480" alt="Untitled (7)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/12cc8d47-b78a-40c7-b479-2270e2f073ba">

                2. **Get 메서드 호출 시 Validation 처리하여 반환**
                    - 예시 코드 : @Override 를 통해 @Getter 로 생성되는 자동 Getter 메서드를 재정의

                     <img width="1000" alt="Untitled (12)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/4cf914a0-87d5-41f6-aa56-fdf9c355161e">


        - **결과적으로는 아래와 같이 Null 및 Validation 처리 로직이 모두 사라짐**

          <img width="1000" alt="Untitled (13)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/00fbc208-8cdf-46a0-8a2a-8b7b74dc7979">



### Exception 공통 처리

위 @RequestBody DTO 객체에서 @Valid 혹은 @Controller 에서의 @Secured 처리 **모두 로직 내 처리 불가**
(@Secured : Spring Security, 로그인한 유저 세션에 할당된 권한 기반의 @Controller 호출 가능여부 처리)

- @Valid 나 @Secured 처럼 추상화된 처리는 **try-catch 구문을 통해 개발자가 처리할 수 없다.**
- @Service 내 발생하는 Exception 모두 **try-catch 하지 않으면** @Controller 를 넘어 클라이언트에 전가
    - 이 두 Exception 케이스의 공통점은 **@Controller 를 넘어 클라이언트에게 전가된다는 것**
        - **이전 :** 이를 처리하기 위해서 일반적으론 @Controller 내 try-catch 구문으로 방어하게된다.
            - 만약 @Controller 로 정의된 API 메서드가 수백개면? **똑같은 try-catch 구문을 수백개 작성**

         <img width="1000" alt="Untitled (14)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/6ab2f602-6c09-4682-b581-517d1b6bd3eb">


        - **이후** : @ControllerAdvice 와 @ExceptionHandler 를 통해 try-catch 를 대신/중앙화
            - @ExceptionHandler 는 어떤 Exception 을 받아서 처리할지 개별 정의가 가능

              : **try-catch 구문에서 catch 에 여러 Exception 들을 if-else 처럼 chaining 하는것처럼**

                - 2가지 방법
                    - @ExceptionHandler({ AException.class, BException.class }) 내 추가
                    - AException a, BException b 파라미터별로 @ExceptionHandler 정의

                      : 단, 이럴 경우 AException 과 BException 이 동일 부모 클래스를 상속받지 않는 이상

              **공통된 필드를 한번에 처리할 수 있는 방법이 없다.** = 중복된 코드가 발생할 수 있다.

            - **추가 1)** `HttpServletRequest` 로 **어떤 유저가, 어떤 URL** 에서 에러 발생한건지 **추적 가능**
            - **추가 2)** @ResponseBody + @ControllerAdvice = **@RestControlerAdvice**
            - **추가 3)** @ControllerAdvice 내 파라미터를 통해 특정 패키지 내 @Controller 에만 적용가능

          <img width="1000" alt="Untitled (15)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/720077bf-b473-47a4-bedd-3dd2fd7b4fca">



### 프론트엔드에게 혹은 API 반환 시 일관된 객체(JSON) 형태로 반환하기

백엔드 API 에서 발행되는 모든 @ResponseBody 는 일관된 객체(JSON) 형태로 가져야 처리에서 추상화 가능

- **이전** : @Controller 내 API 메서드 각자 반환값이 달라서, 프론트엔드에서 각자 달리 처리 필요
    - **백엔드 개발 시 단점**
        - 다양한 Exception 들이 발생하는데, 이들 일일히 정의/반환 불가 (HTTP Status 500 만 가능)
        - 다양한 Exception 들에 대한 상세한 에러 메세지 반환 불가
            - 단, Exception 에 대한 메세지 관리 책임을 백엔드 / 프론트엔드 둘 중 하나로 결정 필요
                - 일반적으로 Exception 은 백엔드에서 발생하기에, 발생지-처리 원칙으로 백엔드가 처리
    - **프론트엔드 개발 시 단점**
        - 값이 반환되지 않은것인지, 에러가 발생한건지 여부 알기힘들다 (제대로 HTTP Status 미반환 시)
        - 백엔드에서 발생한 에러에 대한 메세지를 다원화하기 어렵다.
            - 제대로 HTTP Status 반환한다해도, 유저에게 HTTP Status 기반의 에러 메세지만 발행 가능

  <img width="1000" alt="Untitled (16)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/eb30b9d1-e398-42d6-8aff-ae1f2db389a5">


- **이후** : @Controller 및 @ExceptionHandler 모두 동일한 **RequestResult<T>** 라는 객체를 반환
    - **백엔드** : Exception 에 따른 메세지를 다원화 가능, 성공/실패에 대한 간단한 정적 팩토리 메서드 활용
    - **프론트엔드** : (무지성으로) 백엔드가 보내준 RequestResult 를 통한 에러메세지 및 반환값 간단 처리
        - 프론트엔드는 정말 아무것도 신경쓰지 않고, 화면에 오롯이 집중할 수 있다.

  <img width="1000" alt="Untitled (17)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/622ee8b9-c992-47d1-9a96-bf3b07cf0d59">


    - 아래는 @ControllerAdvice 내 @ExceptionHandler 에서 실패시의 RequestResult 반환

 <img width="1000" alt="Untitled (18)" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/7a412a05-ff64-41ba-8cc8-44c39082bde9">



### DTO 나 Controller 만들때 @FieldDefaults 사용하면 깔끔

매번 필드에 **private final** 붙이는게 귀찮음 → @FieldDefaults 로 정리하여, 빠트릴 실수 방지하기

- @Autowired 를 대체하기 위해 @RequiredArgsConstructor + Final 쓰는데, 그때도 유용

    ```java
    @RequiredArgsConstructor
    @FieldDefaults(makeFinal = true, level = AccessLevel.PRIVATE)
    public class PostService {
    		PostRepository postRepository;
    ```

- 생성자 설정을 위한 Lombok 에서도 **생성자 메서드에 대한 접근자 설정 가능**
    - @NoArgsConstructor(**access = AccessLevel.PROTECTED**)
    - @AllArgsConstructor(**access = AccessLevel.PROTECTED**)

## Spring Cache

### Hibernate(JPA) 교육 시 **2차 캐시에 대한 언급이 있었는데 이에 대한 보완 설명**

저번에 JPA 하면서 1차 캐시가 Persistence Context 라고 얘기하면서, 2차 캐시 얘기를 했는데

- **2차 캐시**는 자동으로 설정되는게 아니라, 우리가 사용하고자하는 **Cache Provider** 를 설정해줘야함

  : Hibernate 구현체 기반으로 설명해보면

    - **1차 캐시 : Session-level Cache** (Transaction-level Cache)
    - **2차 캐시 : SessionFactory-level Cache** (WAS 뜨고, 지는 시간안에 생존 = 어플리케이션 레벨)
    - 1차 및 2차 캐시의 차이점에 대해 **이해가 쉬운 그림 설명**

      ![Untitled (19)](https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/5d28416f-67e5-425d-a5f8-b8c25fb752dd)


     ![Untitled (20)](https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/70d65457-935a-4432-a17d-23e64de5e4b0)


- 2차 캐시 사용을 위해서는 아래 절차를 따라 설정 (Spring Cache 에 대한 설명 - **[참조 1](https://adjh54.tistory.com/165)**, **[참조 2](https://jiwondev.tistory.com/282)**, **[참조 3](https://mangkyu.tistory.com/179)**)
    1. ****@EnableCaching 설정****

        ```java
        @EnableCaching
        @Configuration
        public class CacheConfig {
            ... 
        }
        ```

    2. ****CacheManager Bean 설정****
        - **EhCacheCacheManager** or **ConcurrentMapCacheManager** 등
            - **memcached** 의 경우엔, ConcurrentMap**CacheManager** 사용
            - CacheProvider 어떤걸 쓸까? : Spring 에서 주로 사용되는 **EH**CAC**HE**

              !https://miro.medium.com/v2/resize:fit:1400/1*yihW_Etl_PHiWnzcR84AWw.jpeg

                - Redis 나 Memcached 같은 캐시 엔진들도 있지만, 저 2개의 캐시 엔진과는 달리 ehcache 는 데몬을 가지지 않고 Spring 내부적으로 동작하여 캐싱 처리를 한다.
                - 따라서 Redis 같이 별도의 서버를 사용하여 생길 수 있는 네트워크 지연 혹은 단절같은 이슈에서 자유롭고, 같은 로컬 환경 일지라도 별도로 구동하는 Memcached 와는 다르게 ehcache 는 **서버 어플리케이션과 라이프사이클을 같이 하므로** 사용하기 더욱 간편하다.

              [Spring 로컬 캐시 라이브러리 ehcache](https://medium.com/finda-tech/spring-로컬-캐시-라이브러리-ehcache-4b5cba8697e0)

    3. **Hibernate 에게 2차 캐시를 사용하겠다는 설정**을 알려주어야함
        - `spring.jpa.properties.**hibernate.cache.use_second_level_cache = true**`
        - `spring.jpa.properties.**hibernate.cache.region.factory_class` :** CacheProvider

          : `= org.hibernate.cache.ehcache.**EhCacheRegionFactory` (EhCache 를 사용한다면)**

        - **+ 추가** : `spring.jpa.properties.hibernate.generate_statistics = true`

          : 하이버네이트가 여러 통계정보를 출력하게 해주는데 캐시 적용 여부를 확인할 수 있다.

    4. 2차 캐시에 적재하고자하는 **@Entity 를 지정하여 @Cachable 과 @Cache 어노테이션 모두를 추가**

       *“Entity 저장은 @Cache / 메서드 저장은 @Cachable” 이라고 설명했으나 잘못된 설명이었음 !*

        - **@Cachable** : 캐시의 대상이라는것을 인식시켜주기 위함
        - **@Cache** : CacheConcurrencyStrategy 2차 캐시하는 @Entity 의 용도

          = 2차 캐시된 @Entity 가 동시성 상황에서 어떻게 사용되어야하는가? 에 대한 정의를 내리는것

            - **NONE** : 2차 캐시된 @Entity 에 대해서 아무것도 신경쓰지 않겠다
            - **READ_READ** : 2차 캐시된 @Entity 에 대해 **읽기만 해야한다.**
            - **NONSTRICT_READ_WRITE** : 2차 캐시된 @Entity 에 대해 **읽기/쓰기 다 가능, LOCK 없이**
            - **READ_WRITE** : 2차 캐시된 @Entity 에 대해 **읽기/쓰기 다 가능, SOFT LOCK 으로만 보호**
            - **TRANSACTIONAL** : **견고한 LOCK 적용 = @Transactional**

### @EnableCaching 은 꼭 JPA 의 2차 캐시로만 쓰이는건 아니다. 메소드 캐싱에도 활용 !

스프링은 AOP 방식으로 편리하게 메소드에 캐시 서비스를 적용하는 기능을 제공 (**[참조 1](https://adjh54.tistory.com/165)**, **[참조 2](https://jiwondev.tistory.com/282)**, **[참조 3](https://mangkyu.tistory.com/179)**)

- **캐시 서비스는 트랜잭션과 마찬가지로 AOP를 이용해 메소드 실행 과정에 투명하게 적용**
    - 캐시 관련 로직을 핵심 비즈니스 로직으로부터 분리할 뿐만 아니라, 손쉽게 캐시 기능 적용 가능
- **스프링은 위에 설명했듯, 캐시 구현 기술에 종속되지 않도록 추상화된 서비스를 제공**
    - 그렇기 때문에 환경이 바뀌거나 적용할 캐시 기술을 변경하여도 애플리케이션 코드에 영향을 주지 않는다
- **@Cachable 용례** : 특정 메소드의 결과값을 캐싱할 수 있다.
    - 캐시를 저장 & 조회하기 위한 ****@Cacheable****
        - 메소드의 파라미터 중 원하는것을 Key 값으로, 특정 조건을 만족했을때 캐시하도록 지정 가능
            - 3가지 인자를 주로 사용 (Key 의 경우 없으면 메소드의 파라미터로 설정함)
                - `**value`, `key`, `condition`**
                - `cacheName`, `keyGenerator`, `cacheManager`, `cacheResolver`, `unless`, `sync`

            ```java
            @Slf4j
            @Service
            public class NumberService {
            
                @Cacheable(
            				value = "squareCache",
            				key = "#number",
            				condition = "#number > 10"
            		)
                public BigDecimal square(Long number) {
                    BigDecimal square = BigDecimal.valueOf(number).multiply(BigDecimal.valueOf(number));
                    log.info("square of {} is {}", number, square);
                    return square;
                }
            
            }
            ```

    - 캐시 저장 만을 위한 ****@CachePut****
    - 캐시 제거 만을 위한 ****@CacheEvict****

## Spring Data JPA, Hibernate(JPA)

### Hibernate 전환된 SQL 쿼리 로그

1. `spring.jpa.properties.hibernate.**show_sql**` 은 **System.Out** 에 Log 가 출력
    - `spring.jpa.properties.hibernate.**format_sql**` : **로그 포맷팅**
    - `spring.jpa.properties.hibernate.**highlight_sql**` : **하이라이팅**
    - `spring.jpa.properties.hibernate.**use_sql_comments**` : **주석 포함하여 보기**
2. `logging.level.org.hibernate.**SQL** = DEBUG` 는 **Logger** 에 Log 를 출력 **(꼭 이걸 사용하도록!)**
    - **Logger (SLF4J)** → 어떤 Logger 구현체를 쓸지는 따로 설정이 필요 : **Log4J** or **Logback**
    - `logging.level.org.hibernate.**type.descriptor.sql** = DEBUG` : **바인딩 파라미터 보기**
    - `logging.level.org.hibernate.**type.descriptor.sql.BasicBinder** = DEBUG`
        - 참고로 여기 들어가는 `DEBUG` 에는 `TRACE` 도 들어갈 수 있다. Logger 에서 표기되는 레벨을 뜻함

추가로, 위에 것들은 Hibernate 가 SQL 로 어떻게 전환되는지, JPA 를 보는것이고

- **JdbcTemplate** 는 아래의 설정을 통해 로깅할 수 있다.

```java
logging.level.org.springframework.jdbc.core.JdbcTemplate = DEBUG
logging.level.org.springframework.jdbc.core.StatementCreatorUtils = TRACE
```
