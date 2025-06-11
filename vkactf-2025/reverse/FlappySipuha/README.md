# Задание на реверс .apk
### [FlappySipuha.apk](files/FlappySipuha.apk "Скачать")
#### 1) Закинув приложение в jdax, мы понимаем, что приложение написано не на джаве.
![скрин jadx](files/jadx.png)
#### 2) Распаковываем приложение через apktool.
```
apktool d FlappySipuha.apk
```
![скрин apktool](files/apktool.png)
#### 3) Видим динамическую .so библиотеку, которая вероятнее всего отвечает за логику игры.
![скрин ida pro](files/ida.png)\
Немного проанализировав код мы видим, что приложение принтит какую-то информацию в логи, немного погуглив, мы узнаём о том, что эти логи можно читать через adb logcat
![скрин установки](files/install.png)
#### Устанавливаем на физическое устройство или эмулятор .apk
```
adb logcat -s FlappyBird
```
![скрин логов](files/logging.png)\
Видим логи, ищём проверку в дисассмеблере, будем патчить.
![скрин переходка](files/nopping.png)\
Нашли проверку в дисасме, заменем переход после инструции CMP на NOP
![скрин после нопа](files/after_nopping.png)\
Сохраняем патченную библиотеку, для патчинга использовал плагин patching для ida pro
#### Билдим apk
```
apktool b
```
#### Переподписываем приложение с помощью apksigner
генерация ключа
```
keytool -genkeypair -storetype JKS -v -keystore flappysipuha.jks -keyalg RSA -keysize 2048 -validity 10000 -alias com.flappybird.game
```
подписывание .apk этим ключом
```
apksigner sign --ks flappysipuha.jks --ks-key-alias com.flappybird.game FlappySipuha.apk
```
Устанавливаем приложение, повторяем шаги выше.
![final](files/final.png)
#### Получаем флаг!!!
```
vka{behold_its_flappy_bird_master}
```