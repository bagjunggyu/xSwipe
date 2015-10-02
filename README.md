#####맥북처럼 터치패드로 브라우저를 이용하는 방법입니다.
#####Ubuntu 15.10 GNOME을 MacBook Pro 2015 Retina 13 인치에 설치해서 작업했습니다.
#####xswipe 설정 방법은
#####
#####https://github.com/iberianpig/xSwipe 와 http://blog.mpiannucci.com/view/multitouchgesture 을 참고합니다.
#####
######1. xSwipe 다운받기 
#####
#####$ cd
#####$ wget https://github.com/iberianpig/xSwipe/archive/master.zip
#####$ unzip master.zip
#####
#####또는
#####
#####$ sudo apt install git
#####$ git clone https://github.com/iberianpig/xSwipe.git
###### 
#####2. X11::GUITest 설치하기 & 설치되어 있는 xserver-xorg-input-synaptics 지우고 새로 컴파일 해서 설치하기
#####
#####$ sudo apt install -y libx11-guitest-perl
#####$ sudo apt install -y git build-essential libevdev-dev autoconf automake libmtdev-dev xorg-dev xutils-dev libtool
#####$ sudo apt remove -y xserver-xorg-input-synaptics
#####$ git clone https://github.com/Chosko/xserver-xorg-input-synaptics.git
#####$ cd xserver-xorg-input-synaptics
#####$ ./autogen.sh
#####$ ./configure --exec_prefix=/usr
#####$ make
#####$ sudo make install
#####
#####설치후에는 다시시작해서 터치패드가 제대로 작동하는지 먼저 점검을 합니다.
#####$ sudo reboot
#####
#####이상이 없이 잘 작동하면
#####
#####3. SHMConfig 설정하기
#####/etc/X11/xorg.conf.d/50-synaptics.conf 을 열고 아래의 내용을 붙여넣습니다.
#####
#####$ sudo nano /etc/X11/xorg.conf.d/50-synaptics.conf
#####
#####Section "InputClass"
#####Identifier "evdev touchpad catchall"
#####Driver "synaptics"
#####MatchDevicePath "/dev/input/event*"
#####MatchIsTouchpad "on"
#####Option "Protocol" "event"
#####Option "SHMConfig" "on"
#####EndSection
#####
#####/etc/X11/xorg.conf.d 폴더가 없으면 새로 만들고 위의 내용을 진행합니다.
#####$ sudo mkdir -p /etc/X11/xorg.conf.d
#####
#####설정을 적용하려면 세션을 다시 시작해야합니다. 
#####$ sudo reboot
#####
#####4. xSwipe 테스트 해보기
#####5. $ perl ~/xSwipe/xSwipe.pl
#####
#####중간에, 이런 오류가 나면
#####Can't locate Smart/Comments.pm in @INC (you may need to install the Smart::Comments module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.20.2 /usr/local/share/perl/5.20.2 /usr/lib/x86_64-linux-gnu/perl5/5.20 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.20 /usr/share/perl/5.20 /usr/local/lib/site_perl .) at xSwipe.pl line 15.
BEGIN failed--compilation aborted at xSwipe.pl line 15.
#####
#####패키지 libsmart-comments-perl을 설치하고 다시 해보세요
#####$ sudo apt install libsmart-comments-perl
#####
#####여기까지 문제 없이 되었으면
#####
#####5. 이제 자동 시작에 등록해서 부팅하거나 로그인 할 때마다 자동으로 실행되도록 합니다.
#####우선은 폴더 xSwipe를 숨김폴더 .xSwipe로 이름을 바꿉니다.
#####그리고, 설치를 마친 xserver-xorg-input-synaptics 폴더를 지웁니다.
#####
#####$ cd
#####$ rm -rf xserver-xorg-input-synaptics
#####$ mv xSwipe .xSwipe
#####$ nano ~/.config/autostart/xswipe.desktop
#####그리고 아래 내용을 붙여넣습니다.
#####
#####[Desktop Entry]
#####Type=Application
#####Exec=perl ~/.xSwipe/xSwipe.pl -n -d 0.1 -m 30
#####Hidden=false
#####NoDisplay=false
#####X-GNOME-Autostart-enabled=true
#####Name[ja]=xSwipe
#####Name=xSwipe
#####Comment[ja]=
#####Comment=
#####
#####이제 다시 시작해서 이용합니다.
#####
#####6. 기타 
#####natural scroll (자연 스런 스크롤)을 시스템 설정에서 하지 않고 직접 만드는 방법입니다.
#####
#####$ cd
#####$ nano .Xmodmap
#####
#####pointer = 1 2 3 5 4 7 6 8 9 10 11 12
#####
#####를 붙여 넣습니다.
#####
#####옵션)
#####
-d RATE : RATE is sensitivity to swipe.Default value is 1. Shorten swipe-length by half (e.g.,$ perl xSwipe.pl -d 0.5)
-m INTERVAL : INTERVAL is how often synclient monitor changes to the touchpad state. Default value is 10(ms). Set 50ms as monitoring-span. (e.g.,$ perl xSwipe.pl -m 50)
-n : Natural scroll like Macbook, use "/nScroll/eventKey.cfg".
-e : Enable edge-swipe
#####
#####기타 자세한 설명은 이글을 참고하세요
#####https://github.com/iberianpig/xSwipe
