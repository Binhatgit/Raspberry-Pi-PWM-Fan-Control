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

#Board layout 
```
Fan pin
|.........................|                        
| Raspi chip    1 | 2 +5v |
| |....|          | 4 +5v |-----> +5V
| |....|          | 6 GND |-----> GND
|                 |       |
|                 |       |
|                 |       |
|                 | 18    |-----> Pin 18 Tachometer (BCM 24) of fan
|                 |       |  
|                 |       |
|              35 |       |<----- PWM (BCM 19) of fan, for Fan drive speed
|.........................|

```
#gpio readall
```
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | PHYSICAL | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 |   IN | 1 |  3 || 4  |   |      | 5v      |     |     |
 |   3 |   9 |   SCL.1 |   IN | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 1 |  7 || 8  | 1 | IN   | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | IN   | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 0 | IN   | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |   IN | 0 | 23 || 24 | 1 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |  OUT | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+
```
<img width="941" height="540" alt="image" src="https://github.com/user-attachments/assets/b19dcc25-534f-440b-80e0-67409003603c" />


