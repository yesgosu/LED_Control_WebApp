# LED Remote Control (ESP8266 + Firebase)

Firebase 실시간 데이터베이스를 이용한 IoT LED 원격 제어 시스템

![Arduino](https://img.shields.io/badge/Arduino-00979D?style=flat&logo=Arduino&logoColor=white)
![ESP8266](https://img.shields.io/badge/ESP8266-E7352C?style=flat&logo=espressif&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=Firebase&logoColor=black)

## 📌 프로젝트 개요

이 프로젝트는 ESP8266 마이크로컨트롤러와 Firebase 실시간 데이터베이스를 활용하여 웹 브라우저에서 LED를 원격으로 제어하는 IoT 시스템입니다.

### 주요 특징
- 🌐 **웹 기반 제어**: 브라우저에서 버튼 클릭으로 LED 제어
- ⚡ **실시간 동기화**: Firebase를 통한 즉각적인 상태 반영
- 📱 **크로스 플랫폼**: PC, 스마트폰 등 어디서나 접속 가능
- 🔄 **양방향 통신**: 웹 ↔ Firebase ↔ ESP8266

## 🛠️ 기술 스택

### 하드웨어
- **마이크로컨트롤러**: ESP8266 (NodeMCU)
- **LED**: 일반 LED (D7 핀 연결)
- **전원**: USB 5V

### 소프트웨어
- **펌웨어**: Arduino C++ (ESP8266 Core)
- **프론트엔드**: HTML5, CSS3, Vanilla JavaScript
- **백엔드/데이터베이스**: Firebase Realtime Database
- **통신 프로토콜**: WiFi, HTTPS, WebSocket

### 라이브러리
- `FirebaseArduino.h` - Arduino용 Firebase 라이브러리
- `ESP8266WiFi.h` - WiFi 연결 라이브러리
- Firebase JavaScript SDK v9.5.0

## 📂 프로젝트 구조

```
LED_Control_Arduino/
├── ledweb.ino              # ESP8266 펌웨어 (Arduino 코드)
├── index.html              # 웹 제어 인터페이스
├── on.png                  # LED ON 상태 이미지
├── off.png                 # LED OFF 상태 이미지
└── README.md               # 프로젝트 문서
```

## 🔌 하드웨어 연결

### 회로도
```
ESP8266 (NodeMCU)
├─ D7 → LED(+) → 저항(220Ω) → GND
├─ GND → LED(-)
└─ USB → 5V 전원
```

### 부품 목록
| 부품명 | 수량 | 설명 |
|--------|------|------|
| ESP8266 NodeMCU | 1 | WiFi 내장 마이크로컨트롤러 |
| LED | 1 | 일반 LED (색상 무관) |
| 저항 (220Ω) | 1 | 전류 제한용 |
| 점퍼 와이어 | 3 | 연결용 |
| 브레드보드 | 1 | 회로 구성용 (선택) |

### 연결 방법
1. LED의 긴 다리(+)를 D7 핀에 연결
2. 저항(220Ω)을 LED의 짧은 다리(-)에 연결
3. 저항의 다른 쪽을 GND에 연결

## ⚙️ 설정 및 설치

### 1. Arduino IDE 설정

#### ESP8266 보드 추가
1. Arduino IDE 실행
2. `파일` → `환경설정` → `추가 보드 매니저 URLs`에 추가:
   ```
   http://arduino.esp8266.com/stable/package_esp8266com_index.json
   ```
3. `도구` → `보드` → `보드 매니저`
4. "ESP8266" 검색 → **v2.7.4** 설치 (중요: 버전 확인)

#### 필수 라이브러리 설치
1. `스케치` → `라이브러리 포함하기` → `라이브러리 관리`
2. 다음 라이브러리 설치:
   - **FirebaseArduino** (by Firebase)
   - **ESP8266WiFi** (보드 패키지에 포함됨)

### 2. Firebase 프로젝트 설정

#### Firebase 프로젝트 생성
1. [Firebase Console](https://console.firebase.google.com/) 접속
2. "프로젝트 추가" 클릭
3. 프로젝트 이름 입력 (예: "led-control-project")
4. Google Analytics 설정 (선택사항)

#### Realtime Database 생성
1. 좌측 메뉴에서 "Realtime Database" 선택
2. "데이터베이스 만들기" 클릭
3. 위치 선택 (예: asia-southeast1)
4. 보안 규칙: 테스트 모드로 시작 (나중에 변경 필요)

#### Firebase 정보 확인
1. **데이터베이스 URL**: 
   - Realtime Database 탭에서 확인
   - 예: `https://your-project.firebaseio.com`

2. **API 키 및 설정**:
   - 프로젝트 설정 (⚙️) → "프로젝트 설정"
   - "일반" 탭에서 "웹 앱에 Firebase 추가" 클릭
   - firebaseConfig 정보 복사

### 3. 코드 설정

#### ledweb.ino 수정
```cpp
// Firebase 설정
#define FIREBASE_HOST "your-project.firebaseio.com"  // 본인의 Firebase URL
#define FIREBASE_AUTH "your-database-secret"         // 데이터베이스 비밀번호

// WiFi 설정
#define WIFI_SSID "Your-WiFi-Name"      // 본인의 WiFi 이름
#define WIFI_PASSWORD "Your-Password"    // 본인의 WiFi 비밀번호
```

⚠️ **주의**: 실제 정보로 교체 후 **절대 GitHub에 업로드하지 마세요!**

#### index.html 수정
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:xxxxx",
  measurementId: "G-XXXXXXXXXX",
  databaseURL: "https://your-project.firebaseio.com/"
};
```

### 4. 펌웨어 업로드

1. ESP8266을 USB로 컴퓨터에 연결
2. Arduino IDE에서 보드 설정:
   - `도구` → `보드` → `NodeMCU 1.0 (ESP-12E Module)`
   - `도구` → `Upload Speed` → `115200`
   - `도구` → `포트` → COM 포트 선택
3. `ledweb.ino` 파일 열기
4. 업로드 버튼 클릭 (→)
5. 업로드 완료 후 시리얼 모니터 확인 (Ctrl+Shift+M)

### 5. 웹 인터페이스 실행

1. `index.html` 파일을 웹 브라우저로 열기
2. 또는 웹 서버에 호스팅:
   ```bash
   # Python 간이 서버 (로컬 테스트용)
   python -m http.server 8000
   # 접속: http://localhost:8000
   ```

## 🚀 사용 방법

### 1. 시스템 시작
1. ESP8266에 전원 공급 (USB 연결)
2. 시리얼 모니터에서 WiFi 연결 확인
3. Firebase 연결 확인
4. 웹 브라우저에서 `index.html` 열기

### 2. LED 제어
- **LED 켜기**: 빨간색 "LED ON" 버튼 클릭
- **LED 끄기**: 파란색 "LED OFF" 버튼 클릭
- **상태 확인**: 화면 상단에 현재 상태 표시

### 3. 동작 확인
```
웹에서 버튼 클릭
    ↓
Firebase Database 업데이트
    ↓
ESP8266이 변경 감지 (500ms 주기)
    ↓
LED ON/OFF
    ↓
웹 화면 자동 업데이트
```

## 📊 데이터베이스 구조

### Firebase Realtime Database
```json
{
  "LED_STATUS": "OFF"  // 값: "ON" 또는 "OFF"
}
```

### Firebase Console에서 직접 제어
1. Firebase Console → Realtime Database
2. `LED_STATUS` 값을 클릭하여 수정
3. ESP8266이 즉시 반응

## 🎨 UI 스크린샷

### 웹 인터페이스
```
┌────────────────────────────────────┐
│  LED Remote Control                │
│                                    │
│  LED STATUS : ON                   │
│  (빨간색 텍스트)                    │
│                                    │
│  ┌──────────┐  ┌──────────┐       │
│  │ LED ON   │  │ LED OFF  │       │
│  │ (빨강)   │  │ (파랑)   │       │
│  └──────────┘  └──────────┘       │
│                                    │
│     [LED 켜진 이미지]               │
│                                    │
└────────────────────────────────────┘
```

## 🔒 보안 설정

### ⚠️ 중요: Firebase 보안 규칙 설정

현재 테스트 모드는 **누구나 접근 가능**합니다!

#### 권장 보안 규칙
```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

#### 프로덕션 보안 규칙 (더 안전)
```json
{
  "rules": {
    "LED_STATUS": {
      ".read": true,
      ".write": "auth != null && auth.uid === 'YOUR_USER_ID'"
    }
  }
}
```

### 코드 보안
1. **민감 정보 분리**:
   ```cpp
   // config.h 파일 생성 (.gitignore에 추가)
   #ifndef CONFIG_H
   #define CONFIG_H
   
   #define FIREBASE_HOST "your-project.firebaseio.com"
   #define FIREBASE_AUTH "your-secret"
   #define WIFI_SSID "your-ssid"
   #define WIFI_PASSWORD "your-password"
   
   #endif
   ```

2. **.gitignore 설정**:
   ```
   config.h
   firebase-config.js
   *.bak
   *.tmp
   ```

## 🐛 문제 해결

### WiFi 연결 실패
```
증상: Serial Monitor에 "Connecting..." 무한 반복
해결:
1. WiFi SSID와 비밀번호 확인
2. ESP8266은 2.4GHz만 지원 (5GHz 불가)
3. 라우터가 ESP8266을 차단하는지 확인
```

### Firebase 연결 실패
```
증상: "setting /string failed" 에러
해결:
1. Firebase URL에서 https:// 제거
2. Firebase Auth 키 확인
3. Firebase 보안 규칙 확인
4. 인터넷 연결 확인
```

### LED가 동작하지 않음
```
증상: 코드는 정상이지만 LED 안 켜짐
해결:
1. LED 극성 확인 (긴 다리가 +)
2. D7 핀 연결 확인
3. 저항 연결 확인
4. LED 불량 확인 (다른 LED로 교체)
```

### 웹 페이지에서 버튼 클릭해도 반응 없음
```
증상: 버튼 클릭해도 Firebase 업데이트 안 됨
해결:
1. 브라우저 콘솔(F12) 확인
2. Firebase config 정보 확인
3. Firebase 보안 규칙 확인
4. 인터넷 연결 확인
```

### ESP8266 업로드 실패
```
증상: "espcomm_upload_mem failed" 에러
해결:
1. COM 포트 확인
2. USB 케이블 확인 (데이터 전송 지원 케이블)
3. 업로드 속도 115200으로 변경
4. 보드를 리셋 버튼 누르고 업로드
```

## 💡 활용 사례

- 🏠 **스마트 홈**: 조명 원격 제어
- 🎓 **교육**: IoT 학습 프로젝트
- 🔬 **실험실**: 원격 장비 제어
- 🎮 **취미**: DIY 전자 프로젝트

## 📚 참고 자료

### 공식 문서
- [ESP8266 Arduino Core](https://arduino-esp8266.readthedocs.io/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase JavaScript SDK](https://firebase.google.com/docs/web/setup)
- [NodeMCU Documentation](https://nodemcu.readthedocs.io/)

### 유용한 링크
- [ESP8266 핀아웃](https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/)
- [Firebase Realtime Database](https://firebase.google.com/docs/database)
- [Arduino Reference](https://www.arduino.cc/reference/en/)

### 커뮤니티
- [Arduino Forum](https://forum.arduino.cc/)
- [ESP8266 Community](https://www.esp8266.com/)


## 📝 라이선스

MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

## 👨‍💻 개발자

**yesgosu** - [GitHub](https://github.com/yesgosu)


⭐ 이 프로젝트가 도움이 되셨다면 Star를 눌러주세요!
