# 로드캡슐 (Road Capsule)

> 차와 함께 간 곳에 남긴 한마디를, 다시 그곳에 갔을 때 차가 먼저 꺼내주는 차량용 장소 기억 앱

차는 장소를 기억하고, 목소리는 사람이 남긴다.

현대자동차그룹 차량용 앱 개발 공모전 (Pleos Connect) 출품작

## 프로토타입 안내

현재 저장소의 APK는 **화면 흐름 프로토타입**입니다.
기획한 전체 화면과 이동 흐름을 차량 화면 크기에서 확인할 수 있도록 만든 버전이며, 차량 신호(주행거리, 도어, 좌석)와 Pleos SDK 연동은 개발 예정입니다.

## 핵심 경험

장소 한마디 → 재방문 → 목소리 회상

1. **장소 한마디**: 주차가 확인되면 장소와 상황에 맞는 질문 제시, 짧은 음성으로 기록
2. **함께 남기는 한마디**: 동승자가 있으면 같은 자리에서 함께 기록
3. **재방문 회상**: 같은 장소에 다시 와서 주차하면 그때 남긴 목소리 재생
4. **장소 타임라인**: 한 장소에 쌓인 한마디를 시간순으로 모아보기

보조 기능: 또 오고 싶은 곳 길안내, 주차 메모, 그날의 기억
확장 기능: 차 이름과 프로필, 주행거리 이정표, 미래의 나에게 남기는 편지

## 안전 동작 원칙

| 차량 상태 | 동작 |
| --- | --- |
| 주행 중 | 화면 안내, 알림, 녹음, 재생 없음 (재방문 여부만 내부 판단) |
| 정차 중 (신호 대기 등) | 기록과 재생 없음 |
| 주차 확인 후 | 기록, 재생, 타임라인 열람 |
| 다시 움직이면 | 녹음과 재생 즉시 중단 |

## 필요한 설정

1. Android Studio 설치 (Android SDK 포함)
2. Settings → Languages & Frameworks → Android SDK → SDK Update Sites에 아래 주소 추가
   ```
   https://nexus-playground.pleos.ai/repository/raw-releases/connect-avd/2.x/crp-sys-img.xml
   ```
3. SDK Platforms에서 Pleos Connect 시스템 이미지 설치
4. Device Manager → Create Virtual Device → Automotive 선택
5. 시스템 이미지 "Pleos Connect v2.1.0" 선택, RAM 4GB, Boot option은 Cold boot
6. 별도 권한이나 계정 설정 없이 실행 가능 (프로토타입은 차량 신호를 사용하지 않음)

## 실행 방법

1. Android Studio에서 Pleos Connect Emulator 실행
2. `apk/박시후_로드캡슐_release.apk`를 에뮬레이터 화면에 끌어다 놓거나 아래 명령으로 설치
   ```
   adb install apk/박시후_로드캡슐_release.apk
   ```
3. 앱 목록에서 **로드캡슐** 실행
4. 인트로 후 첫 실행 화면부터 버튼을 눌러 화면 이동
5. 오른쪽 아래 메뉴 버튼으로 전체 화면 목록 이동 가능

## 테스트 환경

| 항목 | 내용 |
| --- | --- |
| 개발 기준 에뮬레이터 | Pleos Connect Emulator 2.1.0 (Android API 34 시스템 이미지) |
| 화면 규격 | 1280 x 720, 가로 화면 |
| 최소 지원 | Android 9 (API 28) |
| 패키지 이름 | com.roadcapsule.app |
| 버전 | 0.3.3 |

## APK 위치

```
apk/박시후_로드캡슐_release.apk
```

## 사용 예정 SDK / API

| SDK | 용도 |
| --- | --- |
| FusedLocation | 장소 도착, 재방문 감지 (근접 알림) |
| Vehicle (Odometer) | 주행거리 변화로 주행과 정지 상태 판단 |
| Vehicle (Door, Seat) | 승하차와 동승자 존재 판단 |
| NaviHelper | 목적지 이름으로 장소 이름 입력, 또 오고 싶은 곳 길안내 |
| SpeechToText | 음성 기록의 텍스트 변환 |
| TextToSpeech | 짧은 안내 음성 |

주차 여부는 단일 신호로 확정하지 않고 주행거리 변화와 도어, 좌석 상태를 조합해 추정하며, 안전 상태가 확인되지 않으면 기록과 재생을 실행하지 않습니다.

## 개인정보

- 음성 녹음과 방문 장소 기록은 첫 실행 시 동의 후 사용
- 동승자는 본인이 직접 참여 선택
- 기본 저장은 차량 내 로컬, 외부 전송 없음
