# SpringStudy

# 스프링
스프링의 핵심 가치는 `객체 지향 프로그래밍` 이라는 것이다.

스프링 프레임워크
* 핵심 기술 : DI, AOP
* 웹 기술 : MVC, WebFlux
* 데이터 접근 : DBC,ORM, XML
* 기술 통합 : 캐시, 이메일, 스케쥴링
* 테스트 : 스프링 기반 테스트 지원

스프링 부트
* 스프링을 편리하게 사용할 수 있도록 지원
* 웹 서버(tomcat)를 내장해서 별도의 웹 서버가 필요하지 않다
* starter 종속성 제공
* 외부 라이브러리 자동 구성

스프링의 핵심
* 자바 언어 기반의 프레임 워크
* 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크

### 객체 지향의 특징
컴퓨터 프로그래밍을 명령어의 목록으로 보는 시각에서
여러개의 독립된 단위, 즉 `객체`들의 *모임*으로 파악
서로 메세지를 주고 받고 데이터를 처리

유연하고 변경이 용이하다
-> 컴포넌트를 쉽고 유연하게 변경하면서 개발하는 방법

## 다형성
## 역할과 구현으로 세상을 구분
운전자와 자동차의 관계를 생각해보자
운전자는 자동차가 어떻게 만들어졌는지 몰라도 운전 가능하고
자동차가 다른 자동차로 바뀌어도 운전이 가능하다
즉, 운전자에게 영향을 주지 않고 변경이 가능하다는 것이다.

공연의 무대도 생각해보면
*로미와 줄리엣*이라는 *역할*이 존재하는 것이므로 해당 배역은 바뀌어도 문제가 되지 않는다.

역할 : 인터페이스
구현 : 인터페이스를 구현한 클래스, 구현 객체
객체 설계 시 역할과 구현을 명확히 분리한다.

자바의 다형성은 *오버라이딩*으로 제공한다

*클라이언트를 변경하지 않고, 서버의 구현 기능을 유연하게 변경할 수 있다.* -> 확장 가능한 설계


## SOLID
1. SRP : 단일 책임 원칙 (Single Responsibility principle)
한 클래스는 하나의 책임만 가진다
책임의 크기에 대한 기준은 *변경*이다.
변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것이다
ex) UI 변경, 객체 생성과 사용을 분리
2. OCP : 개방 폐쇄 원칙 (Open/Closed principle)
소프트웨어 요소는 *확장에는 열려*있으나 *변경에는 닫혀*있어야 한다
*다형성*을 활용하면 된다
인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
객체를 생성하고 연관관계를 맺어주는 별도의 조립, 설정자가 필요하다
3. LSP : 리스코프 치환 형식
객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야한다.
다형성에서 하위 클래스는 *인터페이스 규약을 다 지켜야 한다*.
4. ISP : 인터페이스 분리 원칙
특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다. -> 인터페이스가 명확해지고, 대체 가능성이 높아진다.
사용자 클라이언트 -> 운전자 클라이언트, 정비사 클라이언트
5. DIP : 의존관계 역전 원칙 (dependency Inversion)
추상화에 의존해야지, 구체화에 의존하면 안된다.
구현 클래스에 의존하지 않고, 인터페이스에 의존하라는 뜻
역할에 집중을 하자
```java
private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
private final DiscountPolicy discountPolicy = new RateDiscountPolicy();

위 코드는 OrderServiceImpl의 코드 중 한 부분인데
할인 정책의 변경으로 DiscountPolicy의 구현체들을 변경, 즉 코드를 바꿔주어야한다. 다형성을 통해 가능한 것이다.

하지만 DIP는 위반하고 있다. OrderService는 현재 추상(인터페이스)인 DiscountPolciy를 의존하고 있다(Okay), 하지만 구현체인 Fix,RateDsicountPolicy에도 의존하고 있으므로 DIP를 준수하지 못하고 있다.

뮤지컬이라고 하면 로미오와 줄리엣의 역할이 있는데 로미오가 스스로 줄리엣이 누구인지를 고르고 있는 중이다. DIP를 위반하고 로미오의 역할 외의 다양한 책임을 가지고 있다.
```
```java
private DiscountPolicy discountPolicy;
이 DiscountPolicy의 구현 객체를 대신 생성해주고 주입을 해줘야한다. 
생성자에서는 구현체을 주입받으면 된다.

AppConfig를 만들어준다.
AppConfig는 애플리케이션의 실제 동작에 필요한 *구현객체*를 생성한다.
생성한 객체 인스턴스의 참조를 *생성자를 통해서 주입*한다.

AppConfig는 캐스팅 매니저다. 배우들은 상대 역이 누가 오든 상관이 없어야 하므로 AppConfig가 다 배역을 골라준다.
이제 OrderServiceImpl은 의존관계에 대한 고민은 외부에 맡기고 실행에만 집중하면 된다.

public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService(){
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
하지만 위의 코드에서 Repository의 역할이 잘 보이지 않는다.
리팩토링을 해보자
private static MemoryMemberRepository memberRepository() {
    return new MemoryMemberRepository();
}
new MemoryMemberRepoitory를 생성하게 따로 빼주고
new FixDiscountPolicy로 따로 빼주면 된다.
그러면 요구사항 설계와 잘 맞는다.

```
* *AppConfig*를 통해 관심사를 확실하게 분리했다.
사용영역과 구성영역으로 나누어졌다.

다형성만으로 OCP, DIP를 만족할 수 없다

---


# 제어의 역전 (Inversion of Control)
1. 기존 프로그램은 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고 실행했다. 즉, *구현 객체가 프로그램의 제어 흐름을 스스로 조종*했다.
2. AppConfig를 사용한 이후로, 구현 객체는 *자신의 로직을 실행하는 역할*만 담당한다. 프로그램 제어는 AppConfig가 가져간다.

> 프로그램 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전이라고 한다.

프레임워크 : 내가 작성한 코드에 대한 실행과 제어권을 가지고 있다
라이브러리: 내가 직접 제어의 흐름을 담당한다.

# 의존관계 주입
* 의존관계는 *정적인 클래스 의존관계*,*런타임 시 결정되는 동적인 객체(인스턴스) 의존 관계* 이 두개를 분리해서 생각해야한다.
* 클래스 의존관계 : import 코드만 보고 판단 가능해야 한다.
코드만 보면 해당 구현체에 어떤 클래스가 들어갈지 모른다.
이는 실행 시간에 동적으로 AppConfig를 통해 결정되며 이 때 의존관계가 생성되며, 이를 의존관계 주입이라고 하는 것이다.

IoC, Di Container -> AppConfig 같이 의존성 주입해주는 거

## 스프링 컨테이너
* ApplicationContext : 스프링 컨테이너
* @Bean이 붙은 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록하고, 이를 스프링 빈이라고 한다.
* applicationContext.getBean()을 통해 스프링 빈을 찾는다

빈 팩토리 
* 스프링 컨테이너 최상위 인터페이스
* 스르핑 빈 관리 및 조회
* getBean()

ApplicationContext
* 빈 팩토리를 상속받아 부가적인 기능을 가진다
* 메세지 소스를 활용한 국제화 기능 : 언어 국제화 기능
* 환경변수 : 로컬,개발,운영등을 구분해서 철;
* 애플리케이션 이벤트 : 이벤트 발행,구독 모델을 편리하게 지원
* 편리한 리소스 조회
---
# Singleton Pattern
우리가 여태까지 만들었던 것은 스프링이 없는 순수한 DI 컨테이너다.
즉, 요청을 할 때마다 객체를 새로 할당을 한다
만약 요청이 초 당 100이라면, 초 당 100개의 객체가 새로 생성이 되는 것이며 이는 메모리 낭비를 야기한다.
이를 방지하기 위해 해당 객체를 단 1개만 생성하고, 이를 공유하게 하는 것이다.

> 클래스의 인스턴스가 Only 1개만 생성되는 것을 보장하는 디자인 패턴이다.

문제점
* 의존관계상 클라이언트가 구체 클래스에 의존한다 -> DIP 위반, OCP 위반 가능성이 높아진다
* 내부 속성 변경 및 초기화가 어렵다
* 자식 클래스를 만들기 어렵다
* 유연성이 떨어진다

하지만 스프링 컨테이너를 사용하면 스프링에서 알아서 싱글톤으로 만들어준다.
그래서 이제 요청이 올때마다 객체를 새로 생성하는 것이 아닌 이미 만들어진 것을 재사용해서 효율적으로 사용할 수 있다.

기본 설정은 싱글톤이지만 요청할 때마다 새로운 객체를 만들 수 있게 설정할 수 있다.

## 싱글톤 객체는 상태를 유지하면 안된다. 최대한 stateless하게 설계해야한다.
* 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문이다.
* Therefore
	* 특정 클라이언트에 의존적인 필드가 있으면 안된다
	* 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다
	* 가급적 읽기만 가능해야한다
	* 필드 대신 자바에서 공유되지 않는 ,*지역변수*, *파라메터* ,*쓰레드락* 등을 사용해야 한다.
> 스프링 빈의 필드에서 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다.

스프링은 항상 무상태로 설계를 하자

```java
private int price; // 상태를 유지하는 필드

public void order(String name,int price){
    System.out.println("name = " + name + ", price = " + price);
    this.price = price;
}
--> X
// private int price -> X
public int order(String name,int price){
    System.out.println("name = " + name + ", price = " + price);
    return price;
}
```

AppConfig <- AppConfig@CGLIB
스프링 컨테이너에는 인스턴스 객체가 AppConfig@CGLIB이 등록된다.

* 이 임의의 다른 클래스가 싱글톤이 보장되도록 해준다.
* @Bean이 붙은 메소드마다 이미 스프링 빈이 존재하면 존재하는 빈으로 반환하고, 스프링 빈이 없으면 생서앻서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.

* @Configuration 없이 @Bean만 사용하면 순수한 객체인 AppConfig 객체가 등록이 된다
-> 싱글톤이 깨진다 ( 보장이 안된다) : 의존관계 주입이 필요해서 메소드를 직접 호출할 때
-> 순수한 자바 코드가 들어가는 것이기 때문
*그냥 설정 정보가 있는 곳에는 항상 Configuration을 넣자*

# 하다가 발견한 흥미로운 점
ConfigurationTest를 하다가 강의 처럼 memberRepository가 동일하지 않고 서로 다른 객체로 나왔었다. 문제가 뭐였는지 찾다가 나의 AppConfig 클래스에서 MemberRepository가 static으로 선언이 되어있었다(오타였다)

물론 @Bean을 쓰고 밑에 static을 쓰는 경우는 없을 것이다.
static은 컴파일 타임에 올리는 것이고 @Bean에 등록을 하는 것은 런타임시에 결정이 되는 것이다. 그래서 static으로 된 메소드는 @Bean을 써도 proxy되지가 않아서 우회해서 순수한 자바 객체가 만들어진다.
그래서 의존성 주입이 되지 않으므로 매번 새로운 객체가 나오는 것이다.

---

컴포넌트 스캔과 의존관계 자동 주입
여태까지 우리는 @Bean이나 XML을 통해 설정 정보에 직접 등록할 스프링 빈을 나열했었다.

 스프링에 빈이 많아질수록 반복 코드도 많아지기 때문에 스프링에서 자동으로 빈으로 등록하는 컴포넌트 스캔을 사용해보자.

* @Configuration 아래에 @ComponentScan을 추가하자
여태까지 우리가 다룬 예제에 여러개의 @Configuration이 있는데 이 것도 스캔되면서 등록이 되므로 빼주어야한다

-> `excludeFilters = @ComponentScan.Filter(type= FilterType./ANNOTATION/,classes = Configuration.class)`

* @Component가 붙은 클래스를 스캔해서 Bean에 등록한다
* 우리는 원래 AppConfig에서 의존관계를 주입 했었다. 하지만 이제는 컴포넌트를 통해 자동으로 빈에 올린다. 그러면 의존 관계 주입은 어떻게 할 수 있을 까??

-> 자동 의존 관계 주입이 가능하도록 생성자에 @Autowired 어노테이션을 추가해주면 해결된다.

* 스프링 빈의 기본이름은 클래스명을 사용하되 맨 앞글자만 소문자로 변경한다.
* MemberServiceImpl -> memberServiceImpl
* 직접 지정도 가능하다 @Component(“이름”)

* 생성자에 @Autowired를 지정하면 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다.
* *기본 조회 전략은 타입이 같은 빈*을 찾는다.
* 자동 컴포넌트 스캔 시 basePackages를 통해 스캔 시작하는 위치를 정해줄 수 있다. 없으면 모든 자바 코드를 다 탐색해야한다.
* basePackagesClass를 통해 특정 클래스부터 찾을 수 있다
	* 만약에 클래스를 지정하지 않으면?? -> @ComponentScan을 붙인 패키지부터 시작한다.

> 가장 권장되는 방법은 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것

처음에 프로젝트 만들면 최상단에 있는 CoreApplication이 보일 것이다. 여기에 SpringbootApplication이 있는데 여기에 들어가보면 ComponentScan이 존재한다.


## 컴포넌트 스캔 기본 대상
* @Component : 컴포넌트 스캔에 사용
* @Controller : 스프링 MVC 컨트롤러에서 사용
* @Service: 스프링 비즈니스 로직에서 사용
* @Reposiotry : 스프링 데이터 접근 계층에서 사용
* @Configuration : 스프링 설정 정보에서 사용

자동 빈 등록 vs 자동 빈 등록 -> 충돌
수동 빈 등록 vs 자동 빈 등록 -> 수동 빈이 우선권을 가진다
자동빈을 오버라이딩 해버린다.

의존관계 주입 방법
1. 생성자 주입: 생성자 호출 시점에 1번만 호출되는 것을 보장. 생성자가 단 한개만 있으면 @Autowired를 빼도 된다.
2. 수정자(Setter) 주입 : 원래 필드를 수정할 때는 set필드명으로 쓴다. 여기에 Autowired를 사용하면 된다. 선택, 변경이 가능한 경우에 사용한다.
3. 필드 주입 : 실제 필드에 바로 주입을 시키는 것. 외부에서 변경이 불가능해 테스트하기가 어렵다.
4. 일반 메서드 주입

---
* 주입할 스프링 빈이 없어도 동작해야 할 때가 있다.
1. @Autowired(required = false)로 지정
	* 의존관계가 없기 때문에 메소드 자체가 호출이 되지 않는다.
2. @Nullable : 호출은 되지만 반환 값은 null
3. @Optional<> : Optional.empty
---
## 왜 생성자 주입을 권장하는가?
> 불변
먼저 대부분의 의존관계는 프로그램 종료 전까지 변경하지 않는게 좋다.

수정자 주입으로 의존관계를 주입하면 해당 메소드를 public으로 공개를 해야한다.

> 누락
프레임워크 없이 순수하게 자바 코드로 단위 테스트를 하는 경우 수정자 주입을 사용하면 순수한 자바 코드 테스트에서 각 코드의 의존성을 알아보기 어렵다.

만약에 생성자 주입으로 구성이 되어있다면 컴파일 단에서 오류가 뜰 것이다.

> final 키워드를 사용할 수 있다
final 키워드는 한번만 입력이 가능하므로 생성자 주입을 통해 final 키워드를 사용할 수 있따.
그리고 생성자에서 코드가 누락이 되었다면 내가 해당 생성자 변수를 final로 지정했으면 컴파일 단에서 오류를 확인할 수 있다.

---
Lombok 라이브러리를 사용해서 getter나 setter 코드를 짜지 말자.

* @Getter, @Setter 사용하면 끝!!!
* @RequiredArgsConstructor : 내 클래스 내에 final 변수들을 가지고 생성자를 만들어준다.
	* 이 때 변수 나열하는 거에 유의해야 할 듯. 내가 변수 나열한 대로 생성자가 만들어지기 때문에 다른 클래스에서 호출할 시 해당 순서대로 작성해야한다.
---
## 조회한 빈이 2개 이상일 때
우리가 만들었던 DiscountPolicy를 보면 해당 추상화의 구현체로 RateDiscountPolicy와 FixDiscountPolicy가 있다. 여기서 두 정책을 @Component로 등록을 하면 조회 시 오류가 뜬다.

위 문제를 하위 타입을 직접 지정해서 해결하는 방법이 있겠지만 -> 이것은 DIP를 위반하고 유연성이 떨어진다.

해결방법
1. @Autowired 필드 명 매칭
Autowired는 먼저 *타입 매칭을 시도*하고, 이때 여러 빈이 있으면 *필드 명, 파라메터 명으로 빈 이름을 추가 매칭*한다.

즉 생성자의 파라메터 명을 내가 원하는 클래스의 이름으로 바꾸면 된다. discountPolicy -> rateDiscountPolicy
2. @Qualifier -> @Qualifier끼리 매칭
추가 구분자를 붙여주는 방법. 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것이 아니다.
3. @Primary 사용 : 우선순위를 정하는 방법. 해당 어노테이션이 붙어있는 구현체가 빈으로 선택된다.


코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스 커넥션을 획득하는 스프링 빈이 있다고 할 때, 메인으로 사용하는 것은 @Primary, 서브 데이터베이스는 @Qualifier를 지정해서 깔끔하게 유지할 수 있다.

우선순위의 경우 Quailfier가 더 높다.

* Qualifier(“이름”) 에서 발생할 수 있는 문제점
문자열의 경우 컴파일 타임에서 오류가 검출되지 않기 때문에 이름이 잘못들어가도 찾지 못하는 경우가 있다.

이를 방지하기 위해 해당 이름을 Annotation으로 정의를 하면된다.

그 뒤로 해당 어노테이션이 필요한 클래스나 필드에 @이름을 추가해주면 된다.

Annotation은 상속의 개념이 없다. 그냥 어노테이션을 합쳐서 사용하는 기능은 스프링에서 지원하는 기능이다.
---
## 조회한 빈이 모두 필요할 때, List, Map
위의 예시로 우리가 할인 정책이 2개 다 필요한 경우가  있다.
```java

static class DiscountService {
    private final Map<String, DiscountPolicy> policyMap;
    private final List<DiscountPolicy> policies;

    @Autowired
    public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
        this.policyMap = policyMap;
        this.policies = policies;
        System.out.println("policyMap = " + policyMap);
        System.out.println("policies = " + policies);
    }

    public int discount(Member member,int price ,String discountCode){
        DiscountPolicy discountPolicy = policyMap.get(discountCode);
        return discountPolicy.discount(member, price);
    }
}
Map을 사용해서 의존성 주입을 받을 때 2개의 할인 정책 빈을 모두 받고, 특정 빈을 찾을 때는 Map.get을 통해 찾는다.
```

---
## 자동 vs 수동 빈 등록
먼저 애플리케이션의 업무에 대해서 알아야 한다.
애플리케이션 업무는 크게 2가지로 나누어진다.
1. 비즈니스 로직 빈 : 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리 등이 비즈니스 로직이다. 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.
2. 기술 지원 빈: 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터 베이스 연결, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술이다.

* 비즈니스 로직의 경우, 어느 정도 유사한 패턴이 있어서, 자동 기능을 적극 사용하는 것이 좋다.
* 기술 지원 로직은 수가 적고, 애플리케이션에 전반에 걸쳐서 광범위하게 영향을 미치지만, 적용이 잘 되고 있는 지 파악하기  어려워 가급적 수동 빈으로 등록해서 명확히 드러내는 것이 좋다.

* 물론 비즈니스 로직에서도 다형성을 적극 활용한다면 수동 빈으로 추가하는 경우도 있다. 예를 들어 DiscountService의 코드만 보고 할인 정책에 무슨 정책이 있는 지 한번에 알아차리기가 어렵다. 이럴 때 수동으로 2개의 할인 정책을 등록해서 알아보기 쉽게 할 수 있고, 자동으로 추가할 경우 특정 패키지에 같이 묶어 두는것이 좋다.




