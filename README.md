# Raspberry Pi PWM Fan Control

This is a simple script to control your pwm fan on raspberry pi.

Here's how I wiring the PWM fan on pi:

English: [Using Raspberry Pi to Control a PWM Fan and Monitor its Speed](https://blog.driftking.tw/en/2019/11/Using-Raspberry-Pi-to-Control-a-PWM-Fan-and-Monitor-its-Speed/)

中文：[利用 Raspberry Pi 控制 PWM 風扇及轉速偵測](https://blog.driftking.tw/2019/11/Using-Raspberry-Pi-to-Control-a-PWM-Fan-and-Monitor-its-Speed/)

mkdir -p scripts
mkdir -p scripts/pifancontrol
cd ~ && cd /home/pi/scripts/pifancontrol
chmod +x fan_control.py
chmod +x /usr/local/sbin/fan_control.py

#copy pifancontrol.service to folder etc/systemd/system/
cp fan_control.py /usr/local/bin
cp pifancontrol.service /etc/systemd/system/

sudo systemctl start pifancontrol.service
sudo systemctl status pifancontrol.service

sudo systemctl restart pifancontrol.service

sudo systemctl stop pifancontrol.service

sudo systemctl daemon-reload
sudo systemctl enable pifancontrol.service
sudo systemctl daemon-reload
sudo systemctl enable --no-pager pifancontrol.service
sudo systemctl restart --no-pager pifancontrol.service
sudo systemctl status --no-pager pifancontrol.service

#Config 
nano /usr/local/bin/fan_control.py

#Check temperature
cat /sys/class/thermal/thermal_zone0/temp 
~~~~~~~~~~~~~~~~~~~~~~~
|                     |      Fan
|  Raspi      1  2 +5v|
|             3  4 +5v|----  +5V
|             5  6 GND|----- GND
|             7  8    |
|             9 10    |
~~~~~~~~~~~~~~~~~~~~~~
|    GPIO13  33       |----- Tach
|    GPIO19  35       |------PWM
|_____________________|
