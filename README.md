# linux4

## Задание
Создать R10;
Создать на созданном RAID-устройстве раздел и файловую систему;
Добавить запись в fstab для автомонтирования при перезагрузке.
Сломать 1 из дисков и восстановить RAID-массив;

## Выполнение
### Создаем R10
1. Добавляем 5 виртуальных дисков 
2. Создаем RAID-массив: `sudo mdadm --create --verbose /dev/md0 -l 10 -n 5 /dev/sd{b,c,d,e,f}`
3. Убеждаемся, что у дисков появился раздел md0: `sudo lsblk`  
![](https://sun9-9.userapi.com/impg/97bk-li9K3S36cfjQtjKG42BReGsxpf4tcTauQ/i7ECmMnOaPY.jpg?size=517x309&quality=96&proxy=1&sign=ed8f878a39dd82604f97180ed8901c17&type=album)
4. Создаем конфигурационный файл mdadm.conf:
```
echo "DEVICE partitions" > /etc/mdadm/mdadm.conf
mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf
```  
![](https://sun9-24.userapi.com/impg/EaO72f33KuYzzM2vbZDiF3Q0U1SIC0iKgfIb_w/kQykxwSIykM.jpg?size=995x57&quality=96&proxy=1&sign=a9f2770df5faa48acc6af1e2799259e6&type=album)
### Создаем на Raid-устройстве раздел и файловую систему
1. Создаем файловую систему: `sudo mkfs.ext4 /dev/md0`
2. Монтируем раздел: `sudo mkfs.ext4 /dev/md0`

### Ломаем один из дисков и восстанавливаем RAID-массив
1. Удаляем один виртуальный диск   
![](https://sun9-65.userapi.com/impg/vlW7w5Xb6U2iP9Kk2LTI8BqytnR5aTpNG8uKGQ/-JWguxZreBo.jpg?size=637x202&quality=96&proxy=1&sign=21fa64c3fa8fce607293c36ddcd536d6&type=album)
2. Заменяем наш сломанный диск на другой (у него то же название, но факту это другой диск)
![](https://sun9-13.userapi.com/impg/oJXr2Hg94wjE51Yt9HpUiid4AOIfT1qOZIPqyw/MQNk14wBozQ.jpg?size=660x194&quality=96&proxy=1&sign=20a878cbba7a71f52737342e825cedc2&type=album)
3. Делаем там, чтобы массив оставался после перезагрузки: `sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf`  
![](https://sun9-3.userapi.com/impg/0v52bs_kGl_lqXe4w7RvoX9qBbkkS4FdGboncQ/IwU9mVizQ2M.jpg?size=752x73&quality=96&proxy=1&sign=013936e7c2a8474d9f368323860ef7c7&type=album)
После перезагрузки:    
![](https://sun9-71.userapi.com/impg/GowG94FdWrIGmUs0ZFCTItrYMuJ7Mq3D1Mf79A/A7O1VWcp8V4.jpg?size=643x244&quality=96&proxy=1&sign=a30f35aace2e8c857a2a03bf59ff1ecf&type=album)

### Создаем раздел и файловую систему 
1. Создаем раздел  
![](https://sun9-47.userapi.com/impg/-Jy2rX4R5hHXNAVkgTaK9AeHXSYO8JgwXZhrEw/XCX-z6DwwLU.jpg?size=688x193&quality=96&proxy=1&sign=766d198794e669725fa26f7112a28143&type=album)
2. Создаем файловую систему 
`sudo mkfs.ext4 /dev/md0p1`

### Добавляем запись в fstab
1. Узнаем UUID диска
![](https://sun9-28.userapi.com/impg/khcdn6beMTOd-6ZUcbY9ym-i3uTwLcLQAMEjjg/CkKAtE3Xq-E.jpg?size=593x38&quality=96&proxy=1&sign=9ef27a02a3eb224547cb4955cdc6b224&type=album)
2. Добавляем строку в fstab  
![](https://sun9-19.userapi.com/impg/8tDR7ryFYXgKhLp7kDL7xvrcx0jnt8ttqIee4A/P2mnL2A1XJM.jpg?size=863x310&quality=96&proxy=1&sign=b0de864f13511e6dc5babc88911ce387&type=album)
3. Перезагружаем систему  
![](https://sun9-27.userapi.com/impg/Tpu9EXqP38TsCfEmJo0VhJ5v6LpEBLPSNXY14g/C3rEkul8mZI.jpg?size=539x314&quality=96&proxy=1&sign=5e16b2384982985df54e584157e695a7&type=album)
