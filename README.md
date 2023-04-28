# 프로젝트 명
야! 비켜!

# 개발기간
2023.04.24 ~ 2023.04.28

# 참여인원
3 명

# 개발환경
- Windows 10
- C / C++
- Arduino IDE 2.1 라이브러리 버전
- Python 3.11
- Visual Studio Code
- PyCharm
- openCV 라이브러리
- YOLOv5 객체 탐지 모델
- DRGO 웹캠
- 라인 트레이서(Arduino Nano, L9110S 모터 드라이버, IR 센서, 초음파 센서)

# 주요기능
## 1. 웹캠을 이용한 객체 탐지
- YOLOv5 모델
![a0](/img/a0.png)
- YOLOv5 모델을 이용해서 웹캠 영상 내의 객체들을 탐지 후 'person' 객체만 인식해서 화면 출력 및 라인트레이서에 객체 위치 정보 전송.
![a1](/img/a1.png)
![a2](/img/a2.png)
- 이미지 변환

![b1](/img/b1.png)
![b2](/img/b2.png)
![b3](/img/b3.png)
- 영상을 그레이 이미지로 변환 후, Threshold 기능을 통해 흑백 이미지로 변환 후 값을 반전시킨 후 가장 큰 영역을 라인으로 인식.

## 2. 웹캠과 아두이노 간의 시리얼 통신
- 웹캠은 라인 트레이서와 실시간으로 통신을 하며, 객체가 없을 시 D, 객체가 있을 시 T 신호를 전송
- 웹캠은 라인 트레이서와 실시간으로 통신을 하며, 전방에 있는 라인의 중심점 위치에 따라서 L(좌), R(우), F(전방) 신호 전송

## 3. 딜레이 기능을 통한 라인트레이서의 속도 조절
- analogWrite가 정상 동작하지 않는 관계로 delay함수를 통해 모터를 제어하여 원하는 

## 4. 객체 탐지 시 저장된 동작 수행
- 라인 내의 객체를 탐지하고 라인에서 벗어난 뒤 일련의 사이클을 수행 한 후 다시 라인 방향으로 주행.
- 사이클 수행 후 라인으로 도착했을 경우, 라인 트레이싱 수행.
- 사이클 수행 후 라인으로 도착하지 못했을 경우, 웹캠으로부터 전송받은 라인 중심점 정보를 따라 자동 주행.
- 자동 주행 후 라인으로 도착했을 경우, 라인 트레이싱 수행 이하 반복.

# 작업시간
33시간

# 데모 영상 시연

# 알려진 이슈
## 1. 시리얼 통신으로 인한 딜레이의 존재
- 웹캠과 아두이노 간의 시리얼 통신으로 인해서 동작에 전체적으로 딜레이가 걸리게 되고 딜레이 동안 IR 센서를 통한 라인트레이서가 정상적으로 동작하지 않음.
- ※ 카메라와 아두이노를 시리얼 통신으로 동작하는 것이 아닌 핀을 통해 직접적으로 데이터를 주고 받는 것으로 개선 가능.
- ※ Serial.end()와 Serial.begin()의 조건을 추가해서 원할 때만 시리얼 통신을 하도록 코드를 변경하는 것으로 개선 가능.

## 2. 웹캠의 유선 연결 문제
- 웹캠이 유선을 통해서 데이터를 전송하므로 선 연결의 문제가 있었음.
- 직접 적으로 본체에 웹캠 USB 선을 꽂았을 때 openCV가 비디오를 인식하는 확률은 대략 90%, 연장선을 이용했을 때 인식하는 확률은 대략 10%.
- 동작 도중에도 라인트레이서의 모터가 주는 진동으로 인해서 연결이 해제되는 경우가 많았음.
- ※ 노트북 내장캠, 휴대폰 카메라 등 안정적인 카메라와 블루투스 연결을 통해 데이터를 주고 받아야 함.

## 3. 카메라의 속도와 라인트레이서 구동 속도의 차이로 인한 문제
- DRGO 웹캠은 초당 30프레임을 전송하는데 라인 트레이서의 구동속도가 너무 빠른 나머지 라인이 카메라 인식 범위 밖으로 빠져나가는 현상이 발생.
- ※ 하드웨어적인 성능 개선이 필요 ex) 초당 60프레임 카메라 or 더 넓은 화면을 촬영하는 카메라 or PWM 조절을 통해서 더 부드러운 동작이 가능한 라인트레이서.

## 4. 라인 인식의 문제
- 현재 라인트레이서의 IR 센서는 검은색 절연테이프를 읽도록 설정되어 있으나 바닥의 검은색 자국도 라인으로 인식하는 문제가 있음.
- ※ 1. IR 센서의 가변저항기를 조절하여 센서의 감도를 조절할 수 있다고 함 → 실패
- ※ 2. 자국 패턴과 라인 패턴을 인식시켜서 라인만 인지하도록 해야 함. 
- ※ 3. 라인 조건 변경 : 라인 색을 다르게 하던가 혹은 바탕을 하얀색으로 수정해야 함.
