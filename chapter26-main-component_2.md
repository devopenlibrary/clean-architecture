# 26장 메인(Main) 컴포넌트

모든 시스템에서는 최소한 하나의 컴포넌트가 존재하고, 이 컴포넌트가 나머지 컴포넌트를 생성, 조정, 관리한다.

이 컴포넌트를 메인이라고 부른다.

## 궁극적인 세부사항 

- 메인 컴포넌트는 궁극적인 세부사항으로, 가장 낮은 수준의 정책이다.

- 메인은 시스템의 초기 진입점이다.
    - 특징: 운영체제를 제외하면, 어떤 것도 메인에 의존하지 않는다.
    - 역할
        - 모든 기반 설비를 생성한 후, 시스템에서 더 높은 수준을 담당하는 부분으로 제어권을 넘김
        - 의존성 주입 프레임워크를 이용해 의존성을 주입하는 일
    - 가장 지저분한 컴포넌트 중 하나다.

  - 옵퍼스 사냥 게임의 메인 컴포넌트
      - 주목할 부분: 문자열을 로드하는 방법, 나머지 핵심 영역에서 구체적인 문자열을 알지 못하도록 함

      ```java
      public class Main implements HtwMessageReceiver {
          private static HuntTheWumpus game;
          private static int hitPoints = 10;
          private static final List<String> caverns = new ArrayList<>();
    
          private static final String[] environments = new String[] {
              "bright", "humid", "dry", "creepy", "ugly",
              "foggy", "hot", "cold", "drafty", "dreadful"
          };
    
          private static final String[] shapes = new String[] {
              "round", "square", "oval", "irregular", "long",
              "craggy", "rough", "tall", "narrow"
          };
    
          private static final String[] cavernTypes = new String[] {
              "cavern", "room", "chamber", "catacomb", "crevasse",
              "cell", "tunnel", "passageway", "hall", "expanse"
          };
    
          private static final String[] adornments = new String[] {
              "smelling of sulfur",
              "with engravings on the walls",
              "with a bumpy floor",
              "",
              "littered with garbage",
              "spattered with guano",
              "with piles of Wumpus droppings",
              "with bones scattered around",
              "with a corpse on the floor",
              "that seems to vibrate",
              "that feels stuffy",
              "that fills you with dread"
          };
      }
      ```

  - main 함수
    
    ```java
    public static void main(String[] args) throws IOException {
        game = HtwFactory.makeGame("htw.game.HuntTheWumpusFacade", new Main());
        createMap();
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        game.makeRestCommand().execute();
    
        while (true) {
            System.out.println(game.getPlayerCavern());
            System.out.println("Health: " + hitPoints + "  arrows: " + game.getQuiver());
            HuntTheWumpus.Command c = game.makeRestCommand();
            System.out.println(">");
            String command = br.readLine();
    
            if (command.equalsIgnoreCase("e"))
                c = game.makeMoveCommand(EAST);
            else if (command.equalsIgnoreCase("w"))
                c = game.makeMoveCommand(WEST);
            else if (command.equalsIgnoreCase("n"))
                c = game.makeMoveCommand(NORTH);
            else if (command.equalsIgnoreCase("s"))
                c = game.makeMoveCommand(SOUTH);
            else if (command.equalsIgnoreCase("r"))
                c = game.makeRestCommand();
            else if (command.equalsIgnoreCase("sw"))
                c = game.makeShootCommand(WEST);
            else if (command.equalsIgnoreCase("se"))
                c = game.makeShootCommand(EAST);
            else if (command.equalsIgnoreCase("sn"))
                c = game.makeShootCommand(NORTH);
            else if (command.equalsIgnoreCase("ss"))
                c = game.makeShootCommand(SOUTH);
            else if (command.equalsIgnoreCase("q"))
                return;
    
            c.execute();
        }
    }
    ```
    
    - HtwFactory를 사용해 게임을 생성하는 방식: `htw.game.HuntTheWumpusFacade`라는 메인보다 더 지저분한 클래스를 전달함. 
        - 이는 클래스에서 변경이 생겨도 메인을 재컴파일/재배포 하지 않게 하기 위함
    - 입력 스트림 생성 부분, 게임의 메인 루프 처리, 간단한 입력 명령어 해석 등은 Main함수에서 모두 처리하지만, 실제로는 고수준 컴포넌트로 위임한다.
    
    - 지도 생성 역시 main에서 처리함

        ```java
        private static void createMap() {
            int nCaverns = (int) (Math.random() * 30.0 + 10.0);
            while (nCaverns-- > 0)
                caverns.add(makeName());
    
            for (String cavern : caverns) {
                maybeConnectCavern(cavern, NORTH);
                maybeConnectCavern(cavern, SOUTH);
                maybeConnectCavern(cavern, EAST);
                maybeConnectCavern(cavern, WEST);
            }
    
            String playerCavern = anyCavern();
            game.setPlayerCavern(playerCavern);
            game.setWumpusCavern(anyOther(playerCavern));
      
            game.addBatCavern(anyOther(playerCavern));
            game.addBatCavern(anyOther(playerCavern));
            game.addBatCavern(anyOther(playerCavern));
      
            game.addPitCavern(anyOther(playerCavern));
            game.addPitCavern(anyOther(playerCavern));
            game.addPitCavern(anyOther(playerCavern));
            
            game.setQuiver(5);
        }
        // 이하 코드 생략
        ```


- 요약
    - 메인은 클린 아키텍터에서 가장 바깥 원에 위치하는 지저분한 저수준 모듈
    - 메인은 고수준 시스템을 위한 모든 것을 로드한 후, 제어권을 고수준 시스템에게 넘긴다.

## 결론

메인을 애플리케이션의 플러그인으로 생각하자.
플러그인이므로, 애플리케이션의 설정별로 하나씩 두도록 하여 둘 이상의 메인 컴포넌트를 만들 수도 있다.
- e.g. 개발용 메인 플러그인, 별도의 테스트용 메인 플러그인, 또 다른 상용 메인 플러그인 등

메인을 플러그인 컴포넌트로 여기고 아키텍처 경계 바깥에 위치한다고 보면, 설정 관련 문제를 훨씬 쉽게 해결할 수 있다.
