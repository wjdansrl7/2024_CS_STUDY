# 커맨드(Command) 패턴

# 커맨드 패턴

- 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스로 설계하는 패턴
    - 이벤트가 발생했을 때, 실행될 기능이 다양하면서도 변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 유용하다.
- 행위 패턴의 하나
- 실행될 기능을 캡슐화함으로써 기능의 실행을 요구하는 호출자(Invoker) 클래스와 실제 기능을 실행하는 수신자(Receiver) 클래스 사이의 의존성을 제거한다.
- 따라서 실행될 기능의 변경에도 호출자 클래스를 수정 없이 그대로 사용 할 수 있도록 해준다.
- ⇒ 객체의 행위(메서드)를 클래스로 만들어 캡슐화하는 패턴
- ⇒ 요청을 캡슐화하여 유연성과 확장성을 제공하는 패턴

# 패턴 장점

1. 요청의 캡슐화
    - 요청을 객체로 캡슐화하여 메서드 호출을 매개변수로 전달할 수 있음.
    - 실행할 작업과 작업을 실행하는 객체를 분리
2. 확장성
    - 새로운 커맨드를 쉽게 추가 가능, 커맨드 클래스만 추가하면 되므로 시스템의 확장성 높아짐
3. 히스토리 및 로그 관리
    - 커맨드 객체를 사용하면 실행될 작업의 히스토리를 관리하기 쉬워짐.
    - 커맨드 객체를 통해 작업의 로그를 기록하고 추적할 수 있음.
4. 작업의 취소 및 재실행
    - 커맨드 객체를 사용하면 작업의 취소(Undo)와 재실행(Redo)이 용이, 각 커맨드는 실행 방법과 반대 실행 방법을 포함할 수 있음
5. 명령 큐잉 및 매크로 명령
    - 커맨드 객체를 큐에 저장하여 순서대로 실행할 수 있음.
    - 복잡한 작업을 단일 커맨드 객체로 캡슐화하여 매크로 명령을 구현할 수 있음

# 패턴 단점

1. 복잡성 증가
    - 클래스의 수가 증가한다. 각 작업에 대해 새로운 커맨드 클래스를 만들어야 하므로 코드가 복잡해질 수 있음.
    - 간단한 작업에 대해 너무 많은 클래스가 필요할 수 있음.
2. 디버깅 어려움
    - 커맨드 패턴을 사용할 때, 작업이 객체로 추상화되므로 디버깅이 어려울 수 있음.
    - 어떤 커맨드 객체가 어떤 시점에 실행되었는지 추적하기 어려움
3. 설계의 오버헤드
    - 단순한 시스템에서는 커맨드 패턴을 사용하는 것이 오버헤드가 될 수 있음, 간단한 요청/응답 모델에서는 과도한 설계가 될 수 있음.

<img width="393" alt="Untitled" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/d57090f0-fe96-4f70-9c62-3bd72d1467be">

Command

- 실행될 기능에 대한 인터페이스
- 실행될 기능을 execute 메서드로 선언함

ConcreteCommand

- 실제로 실행하는 기능을 구현
- 즉, Command라는 인터페이스를 구현함

Invoker

- 기능의 실행을 요청하는 호출자 클래스

Receiver

- ConcreteCommand에서 execute 메서드를 구현할 때 필요한 클래스
- 즉, ConcreteCommand의 기능을 실행하기 위해 사용하는 수신자 클래스

# 예시

### (커맨드 패턴 적용 전)

```java
// Light.java
public class Light {
	public void lightOn() {
    	System.out.println("Light on");
    }
}
```

```java
// AISpeaker.java
public class AISpeaker {
	private Light light;
    
    public AISpeaker(Light light) {
    	this.light = light;
    }
    
    public void talk() {
    	light.lightOn();
    }
}
```

```java
// Client.java
public class Client {
    public static void main(String args[]){
    	Light light = new Light();
        AISpeaker aiSpeaker = new AISpeaker(light);
        aiSpeaker.talk();
    }
}
```

만약 불을 켜는 기능 말고, 히터를 트는 기능을 추가하고 싶다면?

```java
// Heater.java
public class Heater {
	public void heaterOn() {
    	System.out.println("Heater on");
    }
}

```

```java
// AISpeaker.java
public class AISpeaker {
	private static String[] modes = {"heater", "light"};
    
	private Light light;
    private Heater heater;
    private String mode;
    
    public AISpeaker(Light light, Heater heater) {
    	this.light = light;
        this.heater = heater;
    }
    
    public void setMode(int index) {
    	this.mode = modes[index];
    }
    
    public void talk() {
    	switch(this.mode) {
        	case "heater":
            	this.heater.heaterOn();
                break;
            case "light":
            	this.light.lightOn();
                break;
        }
    }
}
```

```java
// Client.java
public class Client {
    public static void main(String args[]){
    	Light light = new Light();
        Heater heater = new Heater();
        AISpeaker aiSpeaker = new AISpeaker(light, heater);
        
        aiSpeaker.setMode(0); // heater
        aiSpeaker.talk();
        
        aiSpeaker.setMode(1); //light
        aiSpeaker.talk();
    }
}

```

Heater 클래스를 정의하고, AISpeaker 클래스에서 Heater 객체를 참조하도록 해야함

→ 위와 같이 기능을 추가할수록 객체 프로퍼티는 더욱 늘어날 것이고, talk 메서드의 분기 또한 계속 늘어남

### (커맨드패턴 적용  후)

<img width="739" alt="Untitled 1" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/b7c21481-d3c6-4ebd-888f-f6eacdd67a94">

- AISpeaker가 할 수 있는 기능을 클래스로 만들어 캡슐화
- AISpeaker클래스의 talk 메서드에서 기능을 호출하지 않고, 캡슐화한 Command인터페이스의 메서드를 호출

```java
// Command.java
public interface Command {
    public void run();
}
```

```java
// HeaterOnCommand.java
public class HeaterOnCommand implements Command {
	private Heater heater;
    
	public HeaterOnCommand(Heater heater) {
    	this.heater = heater;
    }
    
    @Override
    public void run() {
    	heater.heaterOn();
    }
}

```

```java
// Heater.java
public class Heater {
	public void heaterOn() {
    	System.out.println("Heater on");
    }
}

```

```java
// LightOnCommand.java
public class LightOnCommand implements Command {
	private Light light;
    
	public LightOnCommand(Light light) {
    	this.light = light;
    }
    
    @Override
    public void run() {
    	light.lightOn();
    }
}
```

```java
// Light.java
public class Light {
	public void lightOn() {
    	System.out.println("Light on");
    }
}
```

```java
/ AISpeaker.java
public class AISpeaker {
	private Command command;
    
    public void setCommand(Command command) {
    	this.command = command;
    }
    
    public void talk() {
    	command.run();
    }
}
```

```java
// Client.java
public class Client {
    public static void main(String args[]){
    	Light light = new Light();
        Heater heater = new Heater();
        
        Command heaterOnCommand = new HeaterOnCommand(heater);
        Command lightOnCommnad = new LightOnCommand(light);
        
        AISpeaker aiSpeaker = new AISpeaker();
        
        aiSpeaker.setCommand(heaterOnCommand);
        aiSpeaker.talk();
        
        aiSpeaker.setCommand(lightOnCommnad);
        aiSpeaker.talk();
    }
}
```

참고자료

[https://gmlwjd9405.github.io/2018/07/07/command-pattern.html](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)

[https://velog.io/@newtownboy/디자인패턴-커맨드패턴Command-Pattern](https://velog.io/@newtownboy/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-%EC%BB%A4%EB%A7%A8%EB%93%9C%ED%8C%A8%ED%84%B4Command-Pattern)’
