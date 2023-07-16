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





