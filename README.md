![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/462ea9b7-021b-4207-bf99-5d7843822675)# Laporan-Resmi-Jarkom-Modul-1-IT25-2024

## Anggota

| Nama                                          | NRP          |
| --------------------------------------------- | ------------ |
| Mohammad Arkananta Radithya Taratugang        | `5027221003` |
| Michael Wayne                                 | `5027221037` |

## >> Evidence
1. Menggunakan filter `frame contains "Login Successful"` untuk melihat email dan password yang benar
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/8db891bb-9721-45bb-a49c-c70a30c26872)
2. Pada packet yang dibuka, terlihat informasi seperti host, server, dll. Didapatkan domain korban = `nanomate-solutions.com` dan web servernya adalah Apache/2.4.56
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/646ab89a-5cc7-41eb-acb0-c575452ed499)

3. Pada stream yang sama, didapati tulisan method POST, pada sebelah kanan method POST tersebut bisa dilihat endpoint yang digunakan user untuk login adalah `/app/includes/process_login.php`

4. Terlihat juga email dan password yang benar, Email: `tareq@gmail.com`, password: `tareq@nanomate`

```
nc 10.15.40.20 10002
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa domain milik korban?
Format: xxxxxx.xxx: e.g. google.com
Jawaban: nanomate-solutions.com
Correct

No 2:
Pertanyaan: Apa web server yang digunakan oleh korban?
Format: name-version: e.g. nginx-1.18.0
Jawaban: apache-2.4.56
Correct

No 3:
Pertanyaan: Apa endpoint yang digunakan untuk login sebagai user biasa?
Format: /path/to/endpoint
Jawaban: /app/includes/process_login.php
Correct

No 4:
Pertanyaan: Apa email dan password yang berhasil digunakan untuk login sebagai user biasa?
Format: email:password
Jawaban: tareq@gmail.com:tareq@nanomate
Correct

Congrats! Flag: JARKOM2024{m4innya_h3bat_xTCkXcptgRVeR8t}
```

## >> Trace Him
1. Cek conversation pada packet dengan Statistics -> Conversations
2. Terlihat bahwa pada protocol `TCP 320` terjadi banyak request/packet yang terkirim dari IP `10.30.3.4`, terlihat juga ada banyak request login yang menunjukkan bahwa itu IP attacker
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/e0aca4e7-d3c0-4530-b22a-36eb02c571f7)
3. Didapatkan IP attacker adalah `10.30.3.4`

```
nc 10.15.40.20 10006
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 6:
Pertanyaan: Alamat IP attacker?
Format: xxx.xxx.xxx.xxx
Jawaban: 10.30.3.4
Correct

Congrats! Flag: JARKOM2024{Wh3re'5_thE_S4uce_uTwkYbxti6dHk89}
```

## >> Secret
1. Gunakan filter `ftp-data` untuk melihat data apa saja yang ada
2. Terlihat ada 2 file yaitu `m4L1c10us_W4re.c` dan `mirza.jpg`
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/e593fc90-86c3-41b5-8f0e-24030e44919c)

3. Untuk dapat melihat isi data, mengunduh kedua file dengan File -> Export Objects -> FTP-DATA -> Save file yang dipilih
![Screenshot 2024-04-02 144537](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/49553894-ff9e-4b64-a35c-c9ef0033d03a)
![Screenshot 2024-04-02 144547](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/9da81a14-5054-4aba-b436-24d08ecb72ed)

4. Didapatkan pesan rahasia attacker adalah `MIO MIRZA`

![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/2f875ba5-670a-4069-b9ec-9cda5857e748)

```
nc 10.15.40.20 10010
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 10:
Pertanyaan: Ternyata attacker menyisipkan file lainnya selain dari file malware, temukan pesan yg dikutip oleh attacker?
Format: strings
Jawaban: MIO MIRZA
Correct

Congrats! Flag: JARKOM2024{l0_Blm_tW_MIO_MIRZA?_cTrkX7xjQzktCAB}
```
   
## >> Fuzz
1. Buka stream

2. Cek conversation pada packet dengan Statistics -> Conversations, terlihat pada protocol `TCP 85` terjadi banyak request yang terkirim dari IP `10.33.1.154`
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/1970647c-4008-465e-b938-b318c3e112b0)

3. Maka didapatkan IP attacker adalah `10.33.1.154`

4. Terlihat juga port dari attacker adalah `80`

5. Gunakan filter `http.request.method == POST`, didapatkan endpoint yang digunakan adalah `/`
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/9880fdce-8531-4213-982f-aee137173829)

6. Follow HTTP stream dari salah satu packet, pada bagian User-Agent terlihat tools yang digunakan adalah Fuzz Faster U Fool v2.0.0-dev
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/2cef4645-d16b-47d6-ae7f-d1cbfac6300c)

7. Berdasarkan stream yang sudah dibuka, kita bisa melihat bahwa banyak percobaan login yang gagal, untuk mencari mana yang berhasil maka bisa menggunakan filter `frame contains "302 Found"`
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/4d0e8160-871b-4966-bfb3-662cb77b0fe1)

8. Follow TCP stream dari packet tersebut, lalu pada kolom Find, bisa dicari kata 302 Found
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/3e6371ca-b441-4a8f-b9bc-6d1552803639)

9. Didapatkan username: admin, dan password: sUp3rSecretp@ssw0rd

```
nc 10.15.40.20 10001
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa IP address milik attacker?
Format: xxx.xxx.xxx.xxx
Jawaban: 10.33.1.154
Correct

No 2:
Pertanyaan: Apa port yang digunakan sebagai web server korban?
Format: xxxx: e.g. 22
Jawaban: 80
Correct

No 3:
Pertanyaan: Apa endpoint yang digunakan untuk login?
Format: /path/to/endpoint
Jawaban: /
Correct

No 4:
Pertanyaan: Apa tool yang digunakan oleh attacker untuk bruteforce login?
Format: toolsname-version: e.g. hydra-v9.0-dev
Jawaban: ffuf-v2.0.0-dev
Correct

No 5:
Pertanyaan: Apa username dan password yang berhasil digunakan oleh attacker?
Format: username:password
Jawaban: admin:sUp3rSecretp@ssw0rd
Correct

Congrats! Flag: JARKOM2024{s3m4ng4t_ya_<3_9hwkXzAHizdtk4t}
```

## >> ATM or ATP or FTP ?

Pada percobaan kali ini praktikan diminta untuk mengetahui password mana yang berhasil dibobol oleh hacker untuk melakukan serangan login.

1. Klik ctrl + F
2. Ubah display filter menjadi string
3. Ketik successful, lalu Enter
4. Muncul paket dengan status response Login successful, lalu klik kanan -> follow -> TCP stream

```
220 Welcome Alpine ftp server https://hub.docker.com/r/delfer/alpine-ftp-server/
USER adminJarkom
331 Please specify the password.
PASS m4y_th3_Kn!fe_ch1p_&_sh4tter
230 Login successful.
```

Telah didapatkan bahwa penyerang menggunakan password "m4y_th3_Kn!fe_ch1p_&_sh4tter" untuk melakukan proses login hingga berhasil

```
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 4:
Pertanyaan: Apa password yang berhasil didapatkan oleh hacker setelah melakukan bruteforce login ftp?
Format: strings
Jawaban: m4y_th3_Kn!fe_ch1p_&_sh4tter
Correct

Congrats! Flag: JARKOM2024{Brut3f0rce_FtP_9hCRPc9jy6Fek8Y}
```

## >> How Many packets?

Pada percobaan kali ini praktikan diminta untuk mengetahui berapa kali hacker melakukan bruteforce.

1. Pada kolom filter ketik ftp.request.command == "PASS"
![image](./images/howManyPackets_1.png)

2. Setelah itu klik Statistics -> Capture File Properties untuk mengetahui nilai packets yang didapat. Tertulis bahwa sebanyak 934 kali hacker melakukan serangan bruteforce <br>
![image](./images/howManyPackets_2.png)

3. Angka 934 dimasukkan ke dalam kolom jawaban dan flag tersebut didapatkan
```
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 5:
Pertanyaan: Berapa total attempt login(bruteforce) yang dilakukan oleh hacker?
Format: number
Jawaban: 934
Correct

Congrats! Flag: JARKOM2024{c0unT_uR_P4cket5_xhCkY7xfl1VtR8Y}
```

## >> Creds

Percobaan kali ini praktikan diminta untuk mencari username dan password yang berhasil dibobol oleh hacker dari salah satu packets yang tersedia.

![image](./images/creds_1.png)

2. Didapat bahwa username dan password tersebut berhasil melakukan login. <br>
![image](./images/creds_2.png)

3. Oleh karena itu, jawaban tersebut dimasukkan oleh pratikan ke dalam pertanyaan berikut
```
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa Username FTP yang digunakan oleh attacker?
Format: USER:username
Jawaban: USER:h3ngk3rTzy
Correct

No 2:
Pertanyaan: Apa Password FTP yang digunakan oleh attacker?
Format: PASS:password
Jawaban: PASS:S!l3ncE
Correct

Congrats! Flag: JARKOM2024{s3curE_uR_FtP_I6wRYbnyQ1koR89}
```

## >> Malwleowleo

Percobaan kali ini praktikan diminta untuk mencari file malware yang dikirim oleh hacker

Terlihat bahwa terdapat 1 file bernama m4L1c10us_W4re.c
![image](./images/malwleowleo_1.png)

```
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 8:
Pertanyaan: Apa nama malware yang dikirim oleh attacker ke korban?
Format: strings
Jawaban: m4L1c10us_W4re.c
Correct

Congrats! Flag: JARKOM2024{beC4refUl_0f_m4lwAr3_u6wkvbAHQ1JtR8q}
```

## >> whoami

Percobaan kali ini praktikan diminta untuk mencari siapa nama hacker tersebut

1. Pada wireshark, klik File -> Export Objects -> FTP-DATA...
2. Terdapat 2 file, yaitu m4L1c10us_W4re.c dan mirza.jpg. Kita pilih file m4L1c10us_W4re.c untuk didownload
3. File yang dibuka akan tampil seperti ini <br>
![image](./images/whoami_1.png)
4. String tersebut kemudian didecode dan menghasilkan output "Hello my name is Paul Atreides". Oleh karena itu, Paul Atreides adalah nama dari hacker tersebut
![image](./images/whoami_2.png)

```
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 9:
Pertanyaan: Siapa nama attacker yang sudah melakukan serangan ini?
Format: FirstName_LastName
Jawaban: Paul_Atreides
Correct

Congrats! Flag: JARKOM2024{Duk3_0f_4rak!s_LISAN AL GHAIB!_ITwkRzxflRFH8rq}
```

## >> Malwaew
1. Karena ketika digunakan filter http, tidak ada packet yang muncul. Maka perlu dilakukan decrypt TLS untuk melihat seluruh komunikasi data yang ada.
![Screenshot 2024-04-02 210205](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/971ee302-b8c7-4090-9851-89cbe9bc52fc)
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/cefc2e50-c7e4-4227-9be0-49d4f262b56c)

2. Lakukan decrypt TLS dengan menggunakan keylog file yang telah diberikan. Decrypt TLS dapat dilakukan dengan Edit -> Preference
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/2f36e8a4-5a6f-477b-859d-79ed49383217)

3 Pada Preferences, dapat dilihat pada bagian Protocol -> TLS, lalu kita dapat memasukkan `keylog.txt` yang telah diberikan pada (Pre)-Master-Secret log filename
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/1c16a62a-1f59-46c4-8083-c0aef528e689)

4. Setelah itu, kita dapat menggunakan filter `http` untuk melihat  POST atau GET. Terdapat 1 file dengan ekstensi .dll yang dimana merupakan executable file dan bisa ditanami malware
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/b65e0727-c1a1-4d06-ba28-ddafabe4770e)

5. Download dan buka file .dll tersebut
![image](https://github.com/radithyaarka/Jarkom-Modul-1-IT25-2024/assets/143694651/ff436108-d89a-4cac-b4b5-ecf22ff9963d)

6. blom tau lanjutannya









