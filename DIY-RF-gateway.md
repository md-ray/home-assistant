# Membuat DIY Bridge/Gateway RF (Radio Frequency) 433 yang terhubung ke Home Assistant

Gelombang RF 433 Mhz adalah teknologi nirkabel yang cukup banyak dipakai di otomasi rumah cerdas, terutama perangkat-perangkat lama (contoh: perangkat sensor rumah dengan alarm GSM). Beberapa penyedia perangkat rumah cerdas sekarang juga masih menggunakan teknologi ini, seperti Sonoff (selain WIFI tentunya). RF 433 memiliki kelebihan jangkauan jarak yang lebih jauh, dan bisa menembus dinding. Namun kelemahannya, RF 433 ini kurang aman jika dibandingkan *wireless* teknologi lainnya.

Kenapa saya membuat RF Bridge yang terhubung ke Home Assistant?
Sederhananya, saya memiliki beberapa sensor lama yang tidak terpakai (sisa dari alarm GSM). Dan tanpa saya duga sebelumnya, ternyata bel rumah saya juga memakai frekuensi 433!

![[Pasted image 20210523123745.png]]

Setiap kali bell rumah dipencet, *event* tersebut bisa ditangkap oleh *home assistant* saya, dan bisa dimanfaatkan sebagai *trigger* untuk membuat otomasi. Misalnya, mengirimkan Telegram message, setiap bel dipencet

![[Pasted image 20210523123812.png]]

[Perangkat yang diperlukan](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Perangkat%20yang%20diperlukan%20564d7dfcb9a2428d82a6e5f80d520f4f.csv)

### Cara membuat

Untuk membuat RF Gateway ini, saya menggunakan software open source, [OpenMQTTGateway](https://docs.openmqttgateway.com/) (OMG). Penggunaannya cukup mudah, dokumentasi sangat lengkap, dan komunitasnya juga aktif. OMG ini di-*flash* di NodeMCU, dan NodeMCU dihubungkan dengan sensor RF433 (SRX882 dan STX882). NodeMCU nanti juga akan terhubung dengan sinyal Wifi di rumah, sehingga bisa terhubung dengan MQTT broker yang sudah disiapkan sebelumnya (saya menggunakan [Mosquitto](https://mosquitto.org/) yang diinstall di Raspberry Pi)

![Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%202.png](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%202.png)

![Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%203.png](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%203.png)

Skema wiring dari dokumentasi official OMG [https://docs.openmqttgateway.com/setitup/rf.html](https://docs.openmqttgateway.com/setitup/rf.html)

Step-by-step nya sepertinya tidak perlu saya tulis ulang di sini, karena dokumentasi dari OpenMQTTGateway sudah sangat lengkap. Kira-kira langkahnya adalah sebagai berikut

1. Hubungkan NodeMCU (atau microcontroller ESP8266 lainnya) ke `SRX882` dan `STX882` mengikuti skema di [https://docs.openmqttgateway.com/setitup/rf.html#esp8266-hardware-setup](https://docs.openmqttgateway.com/setitup/rf.html#esp8266-hardware-setup). Di pengalaman saya, Pin Data di `SRX882`, mesti dihubungkan dengan GPIO `D2` (bukan `D3` sebagaimana default) di NodeMCU
2. Karena saya mesti melakukan modifikasi dari `D3` ke `D2`, maka saya perlu merubah source code OpenMQTTGateway, *compile* ulang dan baru diflash ke NodeMCU. Proses itu semua, dilakukan melalui ArduinoIDE. Silakan instruksi detail di [https://docs.openmqttgateway.com/upload/arduino-ide.html](https://docs.openmqttgateway.com/upload/arduino-ide.html)
3. Melengkapi step no.2 di atas, adapun file-file yang saya download adalah
    1. RF Library: [https://github.com/1technophile/OpenMQTTGateway/releases/download/v0.9.5/nodemcuv2-rf-libraries.zip](https://github.com/1technophile/OpenMQTTGateway/releases/download/v0.9.5/nodemcuv2-rf-libraries.zip)
    2. OpenMQTT Full Source Code: [https://github.com/1technophile/OpenMQTTGateway/releases/download/v0.9.5/OpenMQTTGateway_sources.zip](https://github.com/1technophile/OpenMQTTGateway/releases/download/v0.9.5/OpenMQTTGateway_sources.zip)
4. Ubah configurasi WIFI home network Anda, di file `User_config.h` , supaya nanti NodeMCU bisa langsung connect ke Wifi.
5. Selain itu, atur konfigurasi server MQTT broker anda, juga di file `User_config.h`
6. Hubungkan NodeMCU ke PC/laptop Anda melalui USB, lalu flash OpenMQTTGateway melalui ArduinoIDE
7. Berdoa, semoga berhasil!

![Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%204.png](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%204.png)

Prototype RF433 Gateway menggunakan NodeMCU dan SRX882 dan STX882 yang saya buat

### Pengujian

Bagaimana cara melakukan pengujian apakah rangkaian berhasil? Jika Anda sudah membeli Remote 433 (seperti yang kasih contoh di daftar perangkat di atas), maka setiap tombol remote dipencet, maka akan ada MQTT event yang dipublish di MQTT broker kita. Untuk inspeksi event-event di MQTT broker, Anda bisa menggunakan software [MQTT Explorer](http://mqtt-explorer.com/)

![Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%205.png](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%205.png)

## RF 433 Transmitter

Bagaimana dengan Transmitter RF 433? Ya, tentu saja bisa! Saya bisa membunyikan bel rumah saya dari home-assistant server saya. Cara membuat rangkaian nya, sama dengan apa yang ada di dokumen [https://docs.openmqttgateway.com/setitup/rf.html#esp8266-hardware-setup](https://docs.openmqttgateway.com/setitup/rf.html#esp8266-hardware-setup).

Dari pengalaman saya, saya belum berhasil membuat transmitter bekerja menggunakan komponen STX882 (kemungkinan harus diperbaiki kualitas wiringnya). Akhirnya, saya cari lagi komponen sejenis dan akhirnya ketemu yang bisa, yaitu `FS1000A`.

![Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%206.png](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%206.png)

Prototype RF 433 Transmitter only, menggunakan FS1000A dan Wemos D1 Mini.

### Testing Pengiriman/Transmit RF 433

Cara mengirimkan kode RF433 juga sangat mudah. Anda cukup publish message MQTT ke topic `home/OpenMQTTGateway/commands/MQTTto433` (nama MQTT topic disesuaikan dengan punya anda). Misalnya, saya cukup mengirim MQTT message dengan payload seperti ini untuk membunyikan Bel rumah saya! (merk Kris)

```sql
{"value":5459967,"protocol":1,"length":24,"delay":209, "repeat": 30}
```

## Perangkat dan sensor lainnya yang kompatibel dengan RF433 Gateway ini

Sangat banyak! List lengkapnya bisa dilihat di bawah, atau di [https://docs.google.com/spreadsheets/d/1_5fQjAixzRtepkykmL-3uN3G5bLfQ0zMajM9OBZ1bx0/edit?usp=drive_web&ouid=104229190060678666526](https://docs.google.com/spreadsheets/d/1_5fQjAixzRtepkykmL-3uN3G5bLfQ0zMajM9OBZ1bx0/edit?usp=drive_web&ouid=104229190060678666526)

[RF devices - OpenMQTTGateway compatible](https://compatible.openmqttgateway.com/index.php/devices/rf-433mhz/)

Saya sendiri sudah mencoba 3 device merk DIGOO, yang saya beli dengan harga cukup murah dari bangood

[Untitled](Membuat%20DIY%20Bridge%20Gateway%20RF%20(Radio%20Frequency)%2043%209e2ecb73deb648a6a0462a04d6912125/Untitled%20Database%20c7299cbfc94f4cbd8b7e1b75da6b19dc.csv)