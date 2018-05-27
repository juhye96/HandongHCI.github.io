Raspberry Pi
OS: Raspbian Stretch (Stretch는 version명. 그 이전 version명은 Jessie)

#### 준비물
- Raspberry Pi 세트
	- Raspberry Pi 3
	- mini SD
	- Card Reader
	- HDMI
	- LAN 선
	- ...
- HDMI 모니터
- USB 키보드
- USB 마우스
- 마이크 (Google Speech API나 Alexa를 이용하려면 필요)
- 스피커 (스피커가 지원되는 모니터라면 HDMI 케이블을 이용하여 모니터

#### 다운로드
- www.raspberrypi.org/downloads/raspbian
- 사이트에서 Raspbian with Desktop을 받음(압축 풀지 않음).
- Raspbian Lite는 desktop환경이 빠진 가벼운 버전. Windows와 같은 desktop환경이 구축되므로 파일을 관리하거나 메모장을 이용한 coding 등이 쉬움.

#### 설치
- Mini SD 카드에 Raspbian을 설치 (Windows와 mac에서 모두 시작 가능)
- 설치를 도와주는 tool로 NOOBS, Win32DiskImage, Etcher 등이 있는데, Etcher를 이용하는 것이 advanced된 방법임
- Etcher
	- https://etcher.io 에서 Etcher 프로그램 중 하나를 선택하여 받음 (Windows의 경우 portable 버전을 사용하면 편함)
	- Mini SD에 Raspian 설치
		1. Etcher 실행
		1. 다운로드한 Raspbian with Desktop (Raspbian zip 파일)을 선택 
		1. mini SD 카드가 USB 리더기를 통해 PC의 USB에 꽂고, Etcher에서 해당 드라이브를 선택
		1. Flash를 눌러 Raspbian을 mini SD 카드에 설치함 (10분 이상 소요됨)
		1. 설치 과정에서 mini SD가 몇 개의 파티션으로 나눠지며, 새로 생기는 파티션에 대해 포맷하라는 메시지가 뜨면 포맷을 해주면 됨(exFAT으로 하면 되는지??)
- Raspberry Pi에 Raspbian 설치
	1. Raspberry Pi에 mini SD를 설치하고, 모니터, 키보드, 마우스를 연결한 후 전원을 공급
	1. mini SD에 Raspbian이 제대로 설치가 되었다면 Raspbian의 desktop이 보이게 됨 (혹시 화면이 넘어가지 않고 에러를 표시한다면 Etcher를 이용하여 mini SD에 Raspbian을 덮어 씌워 새로 설치)
	1. 설치가 완료되면 가장 먼저 update를 해주고 불필요한 것들을 지움
	>		sudo apt-get update
	>		sudo apt-get upgrade
	>		sudo apt auto remove
- 기본 설정
	**1. Wifi 연결**
	화면 좌측상단에서 icon을 눌러 wifi를 연결 (만약 아무 wifi가 잡히지 않으면 wifi 설정을 해주어야 됨)
	>		

	**2. Keyboard 입력 변경**
	Preference에 들어가서 Keyboard Layout을 Korean (101/104 key compatible)로 변경

	**3. 한글 폰트 설치**
	>		sudo apt-get install ibus
	>		sudo apt-get install ibus-hangul
	>		sudo apt-get install fonts-unfonts-core

	**4. VIM 설치**
	>		sudo apt-get install vim-gnome
	- VIM 사용법
		- Read only file은 sudo vim 파일명으로 실행
		- a를 누른 후 수정할 영역을 수정
		- 수정이 끝난 후에는 esc를 누른 후, :w로 저장
		- :w!로 강제 저장 (read-only file인 경우)
		- :q로 종료
		- :q!로 저장 없이 강제 종료
		- (파일탐색기에서 해당 파일을 찾은 후 text editor를 이용해서 수정할 수도 있음)

	**5. 마이크 설정**
	>		sudo vim /usr/share/alsa/alsa.conf
	68번째 줄 정도에 있는 아래 부분을
	>		defaults.ctl.card 0
	>		defaults.pcm.card 0
	아래와 같이 변경
	>		defaults.ctl.card 1
	>		defaults.pcm.card 1
	
	**6. 화면을 세로로 회전**
	> 		sudo vim /boot/config.txt
	(앞에 sudo를 붙이면 read-only와 같은 file도 강제 수정할 수 있도록 함)
	config.txt의 가장 아래쪽에 한 줄 추가
	>		display_rotate=1

	**7. 화면보호기/화면자동꺼짐 중지**
	>		sudo vim /etc/X11/xinit/xinitrc
 	9번째 줄 정도에 있는 . /etc/X11/Xsession 아래로 3줄 추가
	>		xset s off
	>		xset -dpms
	>		xset s noblank

	그리고 나서 autostart에도 넣어줌
	>		sudo vim /etc/xdg/lxsession/LXDE/autostart
	아래에 세 줄 추가 (@과 xset은 붙여 씀)
	>		@ xset s off
	>		@ xset -dpms
	>		@ xset s noblank

	**8. 마우스 숨김**
	>		sudo apt-get install unclutter
	>		sudo vim /etc/xdg/lxsession/LXDE/autostart
	가장 아래에 한 줄 추가 (@과 unclutter는 붙여 씀)
	>		@ unclutter -idle 0.1 -root

	**9. NodeJS 최신 버전으로 upgrade**
	NodeJS의 버전을 확인한다.
	>		node -v

	v4.X.X이면 자동 업데이트가 안 되고 지우고 새로 깔아야 한다.
	>		sudo apt-get remove nodered -y
	>		sudo apt-get remove nodejs nodejs-legacy -y
	>		sudo apt-get remove npm -y

	설치할 NodeJS의 버전을 선택하고 설치한다. NodeJS의 버전은 중요하다. npm을 이용하여 node_moduels를 설치할 때 해당 버전의 NodeJS를 이용하게 된다.
	>		curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
	>		sudo apt-get install nodejs -y

	npm은 NodeJS가 설치되면 자동으로 설치된다. node_modules 폴더에 관련 JS들을 설치 및 관리하기 위해서는 아래 것들에 대한 이해가 필요하다.
	>		npm start
	>		npm install
	>		npm remove
	>		npm update
	>		npm cache clean
	>		npm rebuild 

- 참고사이트
	- www.raspberrypi.org/documentation/installation/installing-images/README.md
	- https://wikidocs.net/3163