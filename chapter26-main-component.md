# 26장 메인(Main) 컴포넌트

모든 시스템에는 최소한 하나의 **메인(Main) 컴포넌트**가 존재하며, 이 컴포넌트는 나머지 컴포넌트를 생성하고, 조정하고, 관리한다. 

## 궁극적인 세부사항

메인 컴포넌트는 **가장 낮은 수준의 정책**을 담당한다. 메인은 시스템의 초기 진입점으로서 운영체제를 제외하면 어떤 것도 메인에 의존하지 않는다.

### Main 역할

- 시스템 전반에서 필요한 팩토리(Factory)와 전략(Strategy), 기타 기반 설비를 생성한다.
- 고수준의 시스템 컴포넌트로 제어권을 넘긴다.
- 의존성 주입을 처리한다.
    - 메인에서 의존성을 주입한 뒤에는 DI 프레임워크 없이도 의존성을 분배할 수 있어야 한다.

### Hunt The Wumpus의 Main

**문자열 로드**

문자열 리소스를 한 곳에서 관리

```java
public class Main implements HtwMessageReceiver {
		private static HuntTheWumpus game;
		private static int hitPoints = 10;
		private static fimal List<String> caverns = new ArrayList >();
		
		private static fimal String[] environments = new String[] {
			"bright", "humid", "dry", "creepy" ...
		};
		
		private static fimal String[] shapes = new String[] {
			"round", "square", "oval", "irregular" ...
		};
		
		private static final String[] cavernTypes = new String[] {
			"cavern", "room", "chamber", "catacomb" ...
		};
		
		private static fimal String[] adornments = new String[] {
			"smelling of sulfur",
			"with engravings on the walls",
			"with a bumpy floor",
			...
		};
```

메인은 문자열 리소스를 관리하여 핵심 로직이 문자열의 구체적 형태를 알지 못하도록 한다.

**main 함수**

```java
public static void main(String[] args) throws IOException {
		//  게임 객체 생성 
		game = HtwFactory.makeGame("htw.game.HuntTheWumpusFacade", new Main());
		
		// 지도 생성
		createMap();
		
		// 입력 스트림을 생성
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in) );
		game.makeRestCommand().execute();
		
		// 사용자 입력 해석 / 명령어 생성 실행
		while (true) {
				System.out.println(game.getPlayerCavern());
				System.out.println("Health: " + hitPoints + " arrows: " +
				game.getQuiver());
				
				HuntTheWumpus.Command c = game.makeRestCommand();
				System.out.println(">");

				String command = br.readLine();
				if (command.equalsIgnoreCase("e"))
				
						// 명령 실행은고수준 컴포넌트로 위임
						c = game.makeMoveCommand (EAST);
				else if (command.equalsIgnoreCase("w"))
						c = game.makeMoveCommand(WEST);
				else if (command.equalsIgnoreCase("n"))
						c = game.makeMoveCommand (NORTH) ;
				...
				return;

			c.execute();

		}
}
```

게임 생성 시 htw.game.HuntThewumpusFacade라는 클래스 이름을 전달

- 이 클래스는 메인보다도 더 지저분하기 때문
- 이 클래스에서 변경이 생겨도 메인을 재컴파일/재배포하지 않아도 된다.

메인은 아키텍처의 가장 바깥 원에 위치하는 지저분한 저수준 모듈이다.

역할은 시스템을 초기화하고 고수준 정책에 제어권을 넘기는 것이다.

## 결론

메인은 애플리케이션의 플러그인이다.

초기 조건과 설정을 구성하고, 외부 자원을 모두 수집한 후 제어권을 고수준 정책으로 넘기는 플러그인이다.

메인은 플러그인 개념이므로 필요에 따라 여러 개를 둘 수 있다.

- 개발용 메인
- 테스트용 메인
- 상용 메인
- 국가별/고객별 메인

메인을 플러그인 컴포넌트로 여기고, 그래서 아키텍처 경계 바깥에 위치 한다고 보면 설정 관련 문제를 훨씬 쉽게 해결할 수 있다.