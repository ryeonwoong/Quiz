# <창의공학을 통해서 학생들이 학습해야할 내용에 대해 5가지를 적으시오.>

1. 공학도로서 창의성을 발휘할 수 있도록 한다.
2. 아두이노를 활용하여 IoT 관련 생활에 유용한 제품을 만들 수 있다.
3. 사물 인터넷을 스마트폰 앱으로 무선 제어 할 수 있다.
4. 로봇을 스마트폰으로 구동하고 원하는 위치로 제어할 수 있다.
5. 팀과 협업하고 잘 소통할 수 있는 기초를 다진다.

# <스마트폰으로 집의 전등을 켜고 끄는 시스템을 설계하고 제작하는 과정을 유무선 통신을 포함한 과정을 자세히 설명하시오.>

1. 요구사항 정의: 어떤 전등을 제어할 것인지 정하고 스마트폰 플랫폼을 선택한다.
2. 스마트폰 앱 개발: 선택한 플랫폼에 맞게 앱인벤터와 프로세싱코드 를 이용하여 스마트폰 앱을 개발한다.
3. 무선 통신 설정: 필요한 무선 통신 프로토콜을 구현한다.
4. 릴레이 추가: 릴레이는 전기 장치를 원격으로 제어하기 위한 핵심 부품이다. 릴레이 모듈을 제어 회로에 연결하고 전등을 제어한다.
5. 마무리: 작동이 잘 되는지 확인하고 추가할 기능이 있으면 추가한다.

# <아두이노의 스위치를 읽고 서버로 1초에 1회씩 전송하는 코드와 서버에서 수신하여 1이면 초록색, 2이면 검은색으로 바꾸는 코드를 적으시오.>

# Arduino IDE 코드
```
const int switchPin = 2; // 스위치 핀
int switchState = 0;     // 스위치 상태 (0 또는 1)

void setup() {
  Serial.begin(9600);   // 시리얼 통신 초기화
  pinMode(switchPin, INPUT_PULLUP); // 내부 풀업 저항 사용
}

void loop() {
  // 스위치 상태 읽기
  switchState = digitalRead(switchPin);
  
  // 스위치 상태를 시리얼 포트로 전송
  Serial.println(switchState);

  delay(1000); // 1초 대기
}
```

# Processing 코드
```
import processing.net.*;
import processing.serial.*;

Serial p;
Client c;

void setup() {
  size(200, 200);
  p = new Serial(this, "COM3", 9600);
  c = new Client(this, "192.168.200.132", 80); 
}

void draw() {
  if (arduinoPort.available() > 0) {
    String data = arduinoPort.readStringUntil('\n');
    if (data != null) {
      data = data.trim();
      println("수신된 데이터: " + data);

      // 데이터에 따라 LED 색상 변경
      if (data.equals("1")) {
        background(0, 255, 0); // 초록색
      } else if (data.equals("2")) {
        background(0); // 검은색
      }
  }
}

void keyPressed() {
  if (key == '1' || key == '2') {
    client.write(key); // 키 입력을 서버로 전송
  }
}
```

# <서버에서 스마트폰으로 1을 보내면 스마트폰의 화면이 푸른색으로, 0을 보내면 붉은 색으로 보내는 코드를 적으시오.앱 인벤터의 컴포넌트와 내용을 적으시오.>

# 코드
```
import processing.net.*;

Client c; // 서버와의 클라이언트 연결

void setup() {
  size(400, 400);
  c = new Client(this, "192.168.200.132", 80); // 서버의 IP 주소와 포트를 설정
}

void draw() {
void clientEvent(Client c) {
  String data = c.readString(); // 서버로부터 데이터를 읽습니다.
  if (data != null) {
    data = data.trim();
    
    if (data.equals("1")) {
      backgroundColor = color(0, 0, 255); // 푸른색
    } else if (data.equals("0")) {
      backgroundColor = color(255, 0, 0); // 빨간색
    }
  }
}
}

void keyPressed() {
  if (key == '1' || key == '0') {
    client.write(key); // 키 입력을 서버로 전송
  }
}
```

# 앱인벤터
1. Web 컴포넌트 설정 후 통신: Web컴포넌트를 추가하고 서버 URL을 정한다. 그리고 Web 컴포넌트의 Get 메서드를 사용하여 서버로부터 데이터를 가져온다.
2. 데이터에 따라 배경색 변경: 데이터 수신후, 숫자에 따라 색을 바꾸기 위해 1일때 푸른색 0일때 빨간색이 나오게 한다.
3. 앱디자인: 디자인 화면에서 Canvas 컴포넌트를 추가하고 배경색을 초기화한다.

# <스마트폰에서 on버튼을 누르면 서버를 거쳐 아두이노의 7번핀의 LED가 켜지고, off를 누르면 7번핀의 LED가 꺼진다. 스마트폰의 텍스트 박스에 숫자 1부터8중에 하나를 넣고, Send 버튼을 누르면 아두이노의 4번핀에 달린 피에조 스피커가 도,레,~시,도가 작동되도록 하시오.>

# Processing 코드
```
import processing.net.*;
PImage img;
String serverIP = "192.168.200.132";
int serverPort = 80;

Client client;

void setup() {
  size(200, 200);
  connectToServer();
}

void draw() {
  background(255);
}

void mousePressed() {
  if (mouseX >= 0 && mouseX < width && mouseY >= 0 && mouseY < height) {
    if (img == loadImage("off.png")) {
      img = loadImage("on.png"); // "On" 버튼을 누를 때 이미지 변경
      sendDataToServer("on");
    } else {
      img = loadImage("off.png"); // "Off" 버튼을 누를 때 이미지 변경
      sendDataToServer("off");
    }
    img.resize(width, height);
  }
}

void connectToServer() {
  client = new Client(this, serverIP, serverPort);
}

void sendDataToServer(String data) {
  if (client != null) {
    client.write(data);
    client.write('\n');
  }
}
```

# Arduino IDE 코드
```
const int ledPin = 7; // LED가 연결된 핀 번호
const int buzzerPin = 4; // 피에조 스피커가 연결된 핀 번호

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (Serial.available() > 0) {
    char command = Serial.read();
    
    if (command == 'on') {
      digitalWrite(ledPin, HIGH); // LED 켜기
    } else if (command == 'off') {
      digitalWrite(ledPin, LOW); // LED 끄기
    } else if (command >= '1' && command <= '8') {
      // 피에조 스피커를 사용하여 사운드 재생
      int note = 0;
      switch (command) {
        case '1': note = 261; break; // 도
        case '2': note = 293; break; // 레
        case '3': note = 329; break; // 미
        case '4': note = 349; break; // 파
        case '5': note = 392; break; // 솔
        case '6': note = 440; break; // 라
        case '7': note = 493; break; // 시
        case '8': note = 523; break; // 도
      }
      tone(buzzerPin, note, 500); // 500ms 동안 사운드 재생
    }
  }
}
```

# 앱인벤터

1. 버튼: ON과OFF버튼을 추가한다. LED를 켜고 끄는데 사용
2. Web: 설정에서 URL 설정하기
3. 텍스트상자: 1부터 8까지의 숫자중 하나를 입력할 수 있는 텍스트 상자를 추가한다.
4. Send 버튼: 텍스트 상자에 입력된 숫자를 서버로 보내는 데 사용된다.
5. Send 버튼을 클릭할때 텍스트 상자에 입력된 숫자를 서버로 전송하고, 서버로부터 데이터를 받아서 LED 제어 후, 아두이노로 숫자를 전송하여 피에조 스피커를 제어하는 블록을 작성한다.


