# Daftar Otomasi
Name|Tags|Remarks
-|-|-|
Saya ingin memainkan suara azan di speaker (active speaker, Google Hub) ketika jadwal shalat masuk|`api-shalat`; `speaker`; `time-trigger`|
Saya ingin ada suara bell di active speaker, ketika waktu memasuki jadwal event yang ada di Google Calendar (contoh: jadwal sekolah/ekskul anak)|`google-calendar`; `speaker`; `time-trigger`|
Saya ingin ada suara bell yang diputar di active speaker, setiap jam menit 00, kecuali jam malam ketika tidur|`speaker`; `time-trigger`|
Saya juga ingin mendapatkan notifikasi Telegram, ketika bell rumah dipencet.|`MQTT`; `RF433`; `Telegram`|
Saya ingin memiliki detektor banjir, yaitu ketika dideteksi ada genangan berlebih di kamar mandi, bunyikan sirene (indikasi banjir)|`MQTT`; `RF433`|
Saya bisa menghidup/matikan saklar lampu Sonoff dari mobile apps home assistant|`Sonoff`; `WIFI`|
Saya ingin bisa menghidup/matikan saklar lampu Sonoff secara otomatis, sesuai jadwal / jam|`Sonoff`; `WIFI`; `time-trigger`|
Saya bisa mengirimkan pesan suara (text-to-speech), yang dikirimkan melalui Telegram, diumumkan ke seluruh speaker di rumah|`Telegram`; `speaker`|
Saya bisa melihat informasi terkait suhu, tingkat kelembapan, tekanan udara di dashboard Home Assistant|`Zigbee`|
Ketika saya bekerja dari rumah, saya ingin memiliki sebuah continuous ping response time detector.  Ini untuk mendeteksi jika terjadi gangguan dari Internet Service Provider (ISP). Jika hasil ping melebihi threshold yang di set (misalkan: 50 ms), maka akan dikirimkan alert suara bell yang dimainkan di active speaker, dan notifikasi yang dikirim ke Telegram (TODO: notifikasi tambahan ke LED/smart light)|`Internet`; `Telegram`; `speaker`|
Orang-orang di rumah saya, akan mendapatkan notifikasi ketika akan terjadi hujan di rumah saya. Indikator hujan bisa memanfaatkan data peringatan dini hujan dari BMKG. Notifikasi dikirimkan melalui Telegram dan Bell/pengumumuman di Active Speaker|`Internet`; `Telegram`; `speaker`|
HA memainkan suara notifikasi di active speaker, ketika ada panggilan telepon (biasa dan Whatsapp call) ke HP saya atau istri. Sehingga kami tidak perlu selalu membawa terus HP ketika beraktivitas di rumah|`companion-apps`; `speaker`|
Saya ingin buat reminder yang dibunyikan di active speaker, ketika mesin cuci saya selesai menjalankan task nya (karena bunyi bawaan dari mesin cucinya sangat pelan)|`Zigbee`|