# paho_mqtt_py_demo

주의
==========================

1. usb 연결 시(아두이노 보드, 2d 라이다 센서, depth 카메라), 할당된 시리얼 포트 번호 확인

2. 아두이노 -> 라이다 센서 순으로 연결할 것.

3. 아두이노 연결 포트 테스트 ㉠ :: 

hover_control_yolo_second_arduino_three.ino 파일 실행

A, B, C, D, E, F, G, H, i, j, k 중 하나를 눌러 동작 시험

4. 아두이노 연결 포트 테스트 ㉡ :: 

1 - 동작이 안 될 경우 아두이노 포트 변경(Tools -> Port)

2 - RC_Control_fifo_hover.c 파일의 140 lines ::

handle = open( "/dev/ttyUSB0", O_RDWR | O_NOCTTY ); <-- ttyUSB0을 다른 이름으로 변경 이후 컴파일

3 - paho_mqtt_sub_total_to_motor_test.py 파일의 35 lines ::

port="/dev/ttyUSB0" <-- ttyUSB0을 다른 이름으로 변경

5. 2d 라이다 센서 연결 포트 테스트 ㉠ ::

roslaunch rplidar_ros rplidar.launch 실행

6. 2d 라이다 센서 연결 포트 테스트 ㉡ ::

라이다 동작이 안될 경우

~/catkin_ws/src/RPLidar_Hector_SLAM/rplidar_ros/launch 폴더에서 rplidar.launch 파일 4 lines ::

<param name="serial_port" type="string" value="/dev/ttyUSB1"/>
<-- ttyUSB1을 다른 이름으로 변경

7. 지도 제작 테스트 ::

라이다를 켰으면 roslaunch hector_slam_launch tutorial.launch 실행

8. wifi 접속 환경이 바뀔 경우

다른 wifi 주소를 사용 시 .bashrc 파일에서 130 lines ::

ROS_HOSTNAME=xxx.xxx.x.xx(IP주소) <-- 바뀐 IP주소로 변경

ROS_MASTER_URI=http://xxx.xxx.x.xx.11311 <-- 바뀐 IP 주소(xxx 부분)로 변경

이후 source .bashrc 실행한 다음 명령어를 입력한 터미널 닫고 터미널 다시 열기(프로세스 재시작)

cm(catkin_make) clean 이후 cm(리빌딩)

마지막으로 켜놓고 있던 터미널 전부 종료 후 켜서 라이다 동작 테스트

9. topic 메시지가 남아있어 메시지를 보내지 못하는 경우(mqtt 에러) ::

python3 retained-messages.py 실행

안될 경우 broker 서버를 전부 변경해야 함.

10. 수시로 터미널 확인할 것.

ex) 객체 인식 후 바퀴가 멈추지 않고 계속 회전 시 RC_Control_fifo_hover.c 파일을
실행하여 j를 입력하고 paho_mqtt_sub_total_to_motor_test.py를 실행시킨 터미널을 끄고 다시 켜서 paho_mqtt_sub_total_to_motor_test.py 실행

ex) 카메라가 안 켜질 경우 excute.py를 실행시킨 터미널 끄고 다시 켜서 excute.py 실행

ex) 카메라나 ros가 안 꺼질 경우 exit.py를 실행시킨 터미널 끄고 다시 켜서
exit.py 실행

ex) 위 조치를 실행했는데도 불구, 안 될 경우 모든 프로세스 종료 후 실행 순서대로 실행

실행
==========================

1. (터미널1)처음 paho.mqtt_py_demo_team 폴더 내의 mainScreen.py를 실행

2. (터미널2)excute.py 실행

3. (터미널3)exit.py 실행

4. (터미널4)paho_mqtt_sub_total_to_motor_test.py 실행

5. (터미널5; 선택)바퀴 수동 제어용 RC_Control_fifo_hover.c 실행


알고리즘 설계
===============================

1. mainScreen.py로 pyqt 화면을 열고 버튼을 누를 때 mqtt를 통해서 topic 메시지를 보냄

2. excute.py에서 프로세스를 종료하는 topic 메시지를 일괄적으로 수신

3. exit.py에서 프로세스를 종료하는 topic 메시지를 일괄적으로 수신

4. paho_mqtt_sub_total_to_motor_test.py에서 모터로 송신하는 데이터를 mqtt로 수신 및 모터 정지 데이터를 mqtt로 수신

5. paho_mqtt_pub_depth_to_motor_test <-- 객체 추종

6. paho_mqtt_pub_sub_lidar.py <-- 라이다 동작 및 ros(+hector slam) 실행


