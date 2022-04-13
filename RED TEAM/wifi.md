Wpa:
- PSK - wpaapersonal 
	- Вход по единому паролю, самый распространенный
- enterpprice
	- Стандартизирует процесс аутенфикации, можно написать свой алгоритм. Атака осуществляется вс помощью fakeup.
WPS позволяет не думать о пароле.

airmon-ng перевод карты в режим мониторинга.
```
airmon-ng start wlan0
```
iwconfig - бесповодные адаптеры в системе.


```
airodump-ng mon1
```


__Перевод карты в 13 канал__
```
ifconfig wlan0 down
iw reg set BO
iwconfig wlan0 channel 13
ifconfig wlan0 up
```

Увелечение мощности
```
iwconfig wlan0 txxpower 30
```

aalt+a view modee
alt+s sort
__Смена макадреса__
```
ifconfig wlan0 down
ifconfig wlan0 hw ether MAC
___
macchenger -r wlan0
```
Первые 2 бита показывают измененный или нет