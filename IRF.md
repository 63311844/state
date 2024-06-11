# IRF
วิธีการทำ IRF ของ Switch HPE 5140

 - ใช้ Program SecureCRT 9.4 Version Crack
 - ใช้สาย Link ในการเชื่อม 2 Port
 - สาย Console ใช้ USB to LAN cable

ขั้นตอนดังนี้
1. ต่อ Console ของ Switch ตัวที่ 1 เข้าคอมพิวเตอร์ (กำหนด Port ที่เราจะใช้ ในที่นี้จะใช้ Port 25 และ 26)
2. Config switch ตัวที่ 1 ใน SecureCRT ดังนี้
~~~
system-view
interface range Ten-GigabitEthernet 1/0/25 to Ten-GigabitEthernet 1/0/26
shutdown
quit
~~~
~~~
irf-port 1/1
port group interface Ten-GigabitEthernet 1/0/25
quit

irf-port 1/2
port group interface Ten-GigabitEthernet 1/0/26
quit
~~~
~~~
irf-port-configuration active
~~~
~~~
interface range Ten-GigabitEthernet 1/0/25 to Ten-GigabitEthernet 1/0/26
undo shutdown
~~~
~~~
save
~~~
3. ต่อ Console ของ Switch ตัวที่ 2 เข้าคอมพิวเตอร์ (กำหนด Port ที่เราจะใช้ ในที่นี้จะใช้ Port 25 และ 26)
4. Config switch ตัวที่ 2 ใน SecureCRT ดังนี้
~~~
system-view
irf member 1 renumber 2
save
quit
reboot
~~~
~~~
system-view
interface range Ten-GigabitEthernet 2/0/25 to Ten-GigabitEthernet 2/0/26
shutdown
quit
~~~
~~~
irf-port 2/1
port group interface Ten-GigabitEthernet 2/0/26
quit

irf-port 2/2
port group interface Ten-GigabitEthernet 2/0/25
quit
~~~
~~~
irf-port-configuration active
~~~
~~~
interface range Ten-GigabitEthernet 2/0/25 to Ten-GigabitEthernet 2/0/26
undo shutdown
~~~
~~~
save
~~~
5. ต่อสาย Link โดยต่อไขว้กัน (Port 25 ของ Switch 1 คู่กับ Port 26 ของ Switch 2) และ (Port 26 ของ Switch 1 คู่กับ Port 25 ของ Switch 2)
6. ถ้าทำ IRF สำเร็จ
- Switch จะ reboot ตัวเองและขึ้นไฟโชว์ Port ด้านหน้า Switch
- เช็คโดยใช้คำสั่ง `display interface brief`
