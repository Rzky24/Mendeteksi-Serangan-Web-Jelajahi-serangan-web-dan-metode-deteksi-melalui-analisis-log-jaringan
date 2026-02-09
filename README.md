# Mendeteksi-Serangan-Web-Jelajahi-serangan-web-dan-metode-deteksi-melalui-analisis-log-jaringan

# Pendahuluan
Serangan web adalah salah satu cara paling umum yang digunakan penyerang untuk masuk ke sistem target. Situs web dan aplikasi web yang menghadap publik sering kali berada di depan basis data dan infrastruktur lainnya, yang merupakan target menarik bagi penyerang. Di ruangan ini, Anda akan mempelajari cara mengidentifikasi ancaman ini menggunakan metode deteksi praktis dan alat standar industri

Tujuan
Mempelajari jenis-jenis serangan umum sisi klien dan sisi server
Memahami manfaat dan keterbatasan deteksi berbasis log
Jelajahi metode deteksi berbasis lalu lintas jaringan.
Pahami bagaimana dan mengapa Web Application Firewall digunakan.
Berlatihlah mengidentifikasi serangan web umum menggunakan metode yang telah dibahas.
Prasyarat
Serangan web mencakup berbagai macam teknik. Di ruangan ini, Anda akan mempelajari gambaran singkat beberapa serangan umum sebelum mempelajari cara mendeteksinya. Untuk mendapatkan hasil maksimal dari latihan ini, Anda harus memiliki pemahaman dasar tentang jenis-jenis serangan ini dan sedikit familiaritas dengan menganalisis log dan tangkapan paket

OWASP  Top 10 mencakup sepuluh risiko keamanan web paling kritis.
Baca Pengantar Lengkap  Analisis Log untuk gambaran umum tentang log dan indikator yang berguna.
Wireshark: The Basics  memberikan pengantar yang bagus untuk analisis tangkapan paket.

# Serangan Sisi Klien Client-Side Attacks
Peramban web diserang oleh XSS, tag <script>, dan rudal dengan peringatan yang muncul

Serangan sisi klien mengandalkan penyalahgunaan kelemahan dalam perilaku pengguna atau pada perangkat pengguna. Serangan ini sering mengeksploitasi kerentanan di peramban atau memperdaya pengguna untuk melakukan tindakan yang tidak aman guna mendapatkan akses ke akun dan mencuri informasi sensitif. Data berharga dapat disimpan di perangkat pengguna, sehingga serangan sisi klien yang berhasil dapat mengakibatkan kehilangan data. Dengan meningkatnya permintaan akan aplikasi web yang dinamis dan serbaguna, perusahaan mengintegrasikan lebih banyak plugin pihak ketiga, memperluas permukaan serangan di peramban, dan membuka lebih banyak peluang untuk serangan sisi klien

Bayangkan Anda sedang menjelajahi situs e-commerce favorit Anda dan mengklik gambar produk yang Anda minati. Tanpa Anda sadari, seorang penyerang telah menyembunyikan jendela tersembunyi yang berbahaya di dalam halaman tersebut yang memuat situs lain di latar belakang. Situs tersembunyi ini menjalankan kode berbahaya yang mencuri cookie sesi login Anda. Tidak ada yang tampak aneh, tetapi sekarang penyerang dapat menyamar sebagai Anda dan mengakses akun Anda!

 Keterbatasan SOC
Alat yang tersedia bagi seorang analis, seperti log sisi server dan tangkapan lalu lintas jaringan, menawarkan sedikit atau tidak ada visibilitas tentang apa yang terjadi di dalam browser pengguna. Seperti yang dibahas di atas, serangan sisi klien terjadi pada sistem pengguna, yang berarti mereka dapat mengeksekusi kode berbahaya, mencuri informasi, atau memanipulasi lingkungan tanpa menghasilkan permintaan HTTP atau lalu lintas jaringan yang mencurigakan yang dapat dilihat oleh SOC .  Akibatnya, mendeteksi  serangan ini dari perspektif SOC seringkali sulit atau bahkan tidak mungkin tanpa kontrol keamanan sisi browser tambahan atau pemantauan titik akhir

<img width="1220" height="320" alt="image" src="https://github.com/user-attachments/assets/3596e0bc-ca76-4a40-93e4-4d83fc1a16ab" />

Serangan Sisi Klien Umum
Cross-Site Scripting  ( XSS ) adalah  serangan sisi klien yang paling umum , di mana skrip berbahaya dijalankan di situs web tepercaya dan dieksekusi di browser pengguna. Jika situs web Anda memiliki kotak komentar yang tidak memfilter input, penyerang dapat memposting komentar seperti: Hello <script>alert('You have been hacked');</script>. Saat pengunjung memuat halaman, skrip berjalan di dalam browser mereka, dan pop-up muncul. Dalam serangan nyata, alih-alih pop-up yang tidak berbahaya, penyerang dapat mencuri cookie atau data sesi
Cross-Site Request Forgery ( CSRF ): Browser diperdaya untuk mengirimkan permintaan tidak sah atas nama pengguna tepercaya.
Clickjacking : Penyerang menempatkan elemen tak terlihat di atas konten yang sah, membuat pengguna percaya bahwa mereka berinteraksi dengan sesuatu yang aman.
Jawab pertanyaan di bawah ini
Jenis serangan apa yang bergantung pada eksploitasi perilaku atau perangkat pengguna?

jawaban : Client-Side


Apa serangan sisi klien yang paling umum?

jawaban : XSS

# Serangan Sisi Server
<img width="401" height="146" alt="image" src="https://github.com/user-attachments/assets/97c7b91d-1912-4a03-bc61-64e670b606a2" />


Serangan sisi server  mengandalkan eksploitasi kerentanan dalam server web, kode aplikasi, atau backend yang mendukung situs web atau aplikasi web. Sementara serangan sisi klien memanipulasi interaksi pengguna dengan situs, serangan sisi server berfokus pada pemanfaatan sistem itu sendiri. Dengan mengeksploitasi kelemahan dan kerentanan, logika server, kesalahan konfigurasi, atau penanganan input, penyerang dapat memperoleh akses, mencuri informasi, dan menyebabkan kerusakan pada layanan yang sedang berjalan.

Situs web favorit Anda kemungkinan besar dipenuhi dengan formulir yang memungkinkan input pengguna. Ini bisa berupa formulir login yang menerima nama pengguna atau kata sandi, atau formulir pencarian untuk mencari pesanan sebelumnya atau produk tertentu. Bayangkan jika situs web tersebut salah menangani input dari salah satu formulir ini. Kerentanan ini dapat memungkinkan penyerang untuk mengakses informasi pelanggan atau keuangan sensitif yang tersimpan di basis data backend.

# Menangkap Serangan Sisi Server
Salah satu keuntungan bagi pihak bertahan ketika menghadapi serangan sisi server adalah serangan tersebut meninggalkan jejak bukti, jika Anda tahu di mana mencarinya. Setiap permintaan web yang dikirim ke aplikasi diproses oleh server dan dicatat dalam log atau sistem pemantauan lainnya. Permintaan ini juga berjalan melalui jaringan, yang berarti lalu lintas jaringan dapat mengungkapkan perilaku mencurigakan. Dalam tugas-tugas selanjutnya, kita akan mengidentifikasi serangan sisi server baik dalam log maupun lalu lintas jaringan

<img width="1220" height="320" alt="image" src="https://github.com/user-attachments/assets/094515fa-b7cb-43f0-90c1-a08173a019d7" />

Serangan Sisi Server Umum
Serangan brute-force terjadi ketika penyerang berulang kali mencoba berbagai nama pengguna atau kata sandi dalam upaya untuk mendapatkan akses tidak sah ke suatu akun. Alat otomatis sering digunakan untuk mengirim permintaan ini dengan cepat, memungkinkan penyerang untuk menelusuri daftar kredensial dan kata sandi umum yang panjang.  T-Mobile  menghadapi pelanggaran data pada tahun 2021 yang berasal dari serangan brute-force, yang memungkinkan penyerang mengakses informasi identitas pribadi ( PII ) lebih dari 50 juta pelanggan T-Mobile
SQL  Injection (SQLi) mengandalkan serangan terhadap basis data yang berada di balik sebuah situs web dan terjadi ketika aplikasi membangun kueri melalui penggabungan string alih-alih menggunakan kueri berparameter, sehingga memungkinkan penyerang untuk mengubahSQLdan mengakses atau memanipulasi data. Pada tahun 2023,SQLipadaMOVEit, perangkat lunak transfer file, dieksploitasi, yang memengaruhi lebih dari 2.700 organisasi, termasuk lembaga pemerintah AS, BBC, dan British Airways.
Command Injection adalahserangan umumyang terjadi ketika sebuah situs web menerima input pengguna dan meneruskannya ke sistem tanpa memeriksanya terlebih dahulu. Penyerang dapat menyusupkan perintah, sehingga server dapat menjalankannya dengan izin yang sama seperti aplikasi.
Jawab pertanyaan di bawah ini
Jenis serangan apa yang mengandalkan eksploitasi kerentanan dalam server web?

Server-Side


Serangan sisi server mana yang memungkinkan penyerang menyalahgunakan formulir untuk mengambil isi basis data?

SQLi

# Deteksi Berbasis Log
Log dapat menjadi alat yang berharga untuk mendeteksi serangan web. Setiap permintaan yang dikirim ke server web dapat meninggalkan bukti dalam log akses dan log kesalahan. Para pembela dapat menemukan pola yang mengungkapkan pemindaian, upaya eksploitasi, atau serangan lain dengan meninjau entri log. Dalam tugas ini, Anda akan meninjau dasar-dasar format log akses dan melihat bagaimana berbagai serangan terjadi dalam log akses server web, kemudian mempraktikkan keterampilan Anda dalam skenario urutan serangan nyata.

Format Log Akses

Di bawah ini adalah contoh entri log akses. Tergantung pada konteksnya, setiap kolom dapat menunjukkan lalu lintas yang aman atau berbahaya. Meskipun tidak semua log akses mengikuti format persis ini, umumnya log akses mencakup informasi berikut:

Bidang Log	Contoh Indikator
1. Alamat IP Klien	Berbahaya yang diketahui atau di luar jangkauan geografis yang diharapkan
2. Cap Waktu dan Halaman yang Diminta	Permintaan yang diajukan pada jam-jam yang tidak lazim atau diulang dalam waktu singkat.
3. Kode Status	Respons berulang 404yang menunjukkan halaman tidak dapat ditemukan
4. Ukuran Respons	Jauh lebih kecil atau lebih besar dari ukuran respons normal
5. Perujuk	Mengarahkan ke halaman yang tidak sesuai dengan navigasi situs normal.
6. Agen Pengguna	Versi browser usang atau alat serangan umum (misalnya sqlmap, wpscan)

<img width="1269" height="185" alt="image" src="https://github.com/user-attachments/assets/084a1be9-9850-4a56-afc0-831ade942aba" />

Serangan dalam Log
Selanjutnya, kita akan memeriksa contoh urutan serangan yang dipadatkan. Urutan entri log hanya akan menampilkan permintaan penyerang; namun, dalam skenario dunia nyata, lalu lintas yang tidak berbahaya akan membentuk sebagian besar entri log, sehingga penting untuk mengembangkan kemampuan pengamatan yang tajam untuk menemukan pola berbahaya di tengah aktivitas normal. Penting juga untuk dicatat bahwa dalam contoh di bawah ini, string kueri dalam serangan SQLi dicatat, sehingga Anda dapat melihat muatan SQLi lengkap

Penyerang menguji potensi direktori dan formulir untuk dieksploitasi dengan menggunakan metode directory fuzz. 200Kode respons menunjukkan temuan yang valid bagi penyerang.
Selanjutnya, penyerang mengeksploitasi login.phpformulir yang ditemukan dengan serangan brute-force. Perhatikan POSTpermintaan berulang yang terjadi dengan cepat. Permintaan terakhir POSTmemiliki kode respons yang berbeda, 302 Found, yang dalam hal ini menandakan upaya login yang berhasil. Halaman kemudian dialihkan ke halaman akun pengguna di /account.
Setelah penyerang mendapatkan akses ke akun, mereka mencoba dua payload SQLi, ' OR '1'='1dan 1' OR 'a'='a, pada /searchformulir tersebut. Jika aplikasi membangun kueri SQL secara dinamis alih-alih menggunakan kueri berparameter, penyerang dapat melakukan dump basis data.

<img width="1390" height="685" alt="image" src="https://github.com/user-attachments/assets/e67b73fc-3c91-4ef3-a0d7-fdd9d8e6ca4e" />
Keterbatasan Log
Meskipun log akses berguna, log tersebut tidak selalu mencatat seluruh isi permintaan, terutama isi  POST atau  GET permintaan. Misalnya, upaya login dapat muncul di log akses sebagai:

10.10.10.100 [12/Aug/2025:14:32:10] "POST /login HTTP/1.1" 200 532 "/home.html" "Mozilla/5.0"

Contoh log di atas menunjukkan metode permintaan, halaman yang diminta, dan kode status, tetapi bukan kredensial atau muatan sebenarnya yang dikirimkan dalam permintaan. Yang penting, log akses tidak mencatat POSTdata isi sebenarnya, seperti kredensial yang dikirimkan, sehingga penyelidik hanya dapat melihat bahwa permintaan telah terjadi.  GETLog dapat mencatat jalur lengkap dan string kueri, tetapi beberapa format log tidak akan menyertakannya sama sekali. Ini sepenuhnya bergantung pada perangkat lunak server yang digunakan dan konfigurasi pencatatan log.

Investigasi
TryBankMe, sebuah platform perbankan online kecil, telah mengalami pelanggaran keamanan. Penyerang masuk dan membocorkan data pelanggan yang sensitif di forum darknet. Manajemen meyakini bahwa intrusi dimulai melalui situs web publik perusahaan, dan terserah Anda untuk mengungkap bagaimana tepatnya hal itu terjadi

Mulailah dengan membuka access.logfile di desktop. 

Misi Anda adalah menganalisis log dan menelusuri kembali langkah-langkah penyerang untuk mengungkap bagaimana pelanggaran tersebut terjadi. Semoga berhasil!

Jawab pertanyaan di bawah ini
Apa User-Agent penyerang saat melakukan fuzzing direktori?

buka contoh access log - lihat detail permintaan GET BERULANG KE BERBAGAI ENDPOINT
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5c367389-83c7-4d08-96c5-d7012be5924e" />



jawaban : FFUF v2.1.0


Apa nama halaman tempat penyerang melakukan serangan brute-force?
cara sama seperti yang di atas tinggal mencari : /login.php
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/94ea53b2-dbbe-48dc-be7b-0bf36677569e" />

jawaban : /login.php


Apa muatan SQLi lengkap yang telah didekode yang digunakan penyerang pada /changeusername.phpformulir tersebut?
kita harus menyalin sqli yang di codekan di cyber cheff ini yang harus di code kan :192.168.1.10 - - [20/Aug/2025:07:38:20 +0000] "GET /account/changeusername.php?q=%25%27+OR+%271%27%3D%271 HTTP/1.1" 200 289 "sqlmap/stable" 

lalu kita buka cyber cheff pilih url decode : <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d4da5724-2678-4849-9155-f493df709196" />



<img width="1335" height="669" alt="image" src="https://github.com/user-attachments/assets/5dd3bd3f-73c0-4d6b-bdc4-9b43e47f3789" />
 


jawaban : %' OR '1'='1


eteksi Berbasis Jaringan
Analisis lalu lintas jaringan memungkinkan analis untuk memeriksa data mentah yang dipertukarkan antara klien dan server. Dengan menangkap dan memeriksa paket, analis dapat mengamati perilaku serangan pada tingkat yang lebih detail, termasuk protokol transport yang mendasarinya dan data aplikasi itu sendiri. Tangkapan jaringan lebih rinci daripada log server, mengungkapkan data di balik setiap permintaan dan respons. Ini dapat mencakup header HTTP lengkap , isi POST, cookie, dan file yang diunggah dan diunduh, misalnya.

Protokol yang mengandalkan enkripsi, seperti HTTPS dan SSH , membatasi apa yang dapat dilihat dalam muatan paket tanpa akses ke kunci dekripsi. Untuk contoh kita, kita akan fokus secara khusus pada lalu lintas HTTP dalam tugas ini.

Serangan pada Lalu Lintas Jaringan
Mari kita mulai dengan meninjau urutan serangan dari tugas sebelumnya dan membandingkan tampilannya di Wireshark. Tersedia banyak filter berbeda untuk menyoroti bidang yang Anda minati. Di bawah ini, kami telah memfilter alamat IP tujuan 10.10.20.200dan User-Agent menggunakan http.user_agentfilter tersebut.

Ingat kembali urutan kejadiannya.

Pencarian direktori secara acak untuk menemukan direktori atau formulir yang valid.
Upaya serangan brute-force dengan paket 13 sebagai login yang berhasil.
Upaya injeksi SQL

<img width="1336" height="448" alt="image" src="https://github.com/user-attachments/assets/361328b0-353b-45fe-9b75-b3476d8a97f1" />
Setelah kita memeriksa urutan kejadian, mari kita periksa detail paket untuk mengumpulkan bukti lebih lanjut dari serangan tersebut. Pertama, melihat upaya brute force. Jika Anda ingat, log menunjukkan POSTpermintaan berulang ke login.phpformulir. Sekarang kita dapat melihat nama pengguna dan kata sandi sebenarnya yang digunakan penyerang untuk mencoba masuk. Memeriksa permintaan terakhir ke formulir (paket 13), yang kita tahu berhasil, kita dapat melihat bahwa penyerang menemukan kata sandi yang valid password123. Bukan kata sandi yang sangat aman, terutama untuk akun admin!

<img width="1000" height="446" alt="image" src="https://github.com/user-attachments/assets/601c95d5-2029-43d4-a6b1-d3dcc028b4fe" />

Selanjutnya, mari kita lihat upaya SQLi pertama dari serangan tersebut secara lebih detail. Memeriksa paket HTTP memungkinkan kita untuk melihat dengan jelas muatan (payload) yang digunakan oleh penyerang dan hasil serangannya. Dalam kasus ini, ' OR '1'='1muatan tersebut memungkinkan tabel Pengguna (Users) untuk diekspos, menampilkan Nama Depan dan Nama Belakang dalam teks biasa untuk penyerang! Perlu dicatat bahwa lalu lintas protokol MySQL juga dapat dianalisis menggunakan Wireshark dan menunjukkan muatan serta hasil yang dikembalikan.

<img width="1009" height="520" alt="image" src="https://github.com/user-attachments/assets/6158a30c-328f-4cf4-9915-df70c2649f36" />

Investigasi Berlanjut
Meskipun analisis log Anda menemukan beberapa bukti serangan, Anda tidak dapat melihat pengguna mana yang diretas atau data apa yang sebenarnya dicuri. Untungnya, kami memiliki tangkapan lalu lintas jaringan saat serangan terjadi. Telusuri file traffic.pcapdi desktop pengguna untuk melanjutkan investigasi Anda. Saat Anda menelusuri lalu lintas jaringan, ingatlah urutan serangan dari investigasi Anda sebelumnya. Semoga berhasil!

Tips: 

Gunakan httpfilter di Wireshark untuk hanya melihat lalu lintas HTTP

Anda juga dapat mengklik kanan pada paket mana pun → ikuti Aliran HTTP untuk merekonstruksi permintaan dan respons lengkap antara klien dan server.

Jawab pertanyaan di bawah ini
Kata sandi apa yang berhasil diidentifikasi oleh penyerang dalam serangan brute-force?

Investigasi Berlanjut
Meskipun analisis log Anda menemukan beberapa bukti serangan, Anda tidak dapat melihat pengguna mana yang diretas atau data apa yang sebenarnya dicuri. Untungnya, kami memiliki tangkapan lalu lintas jaringan saat serangan terjadi. Telusuri file traffic.pcapdi desktop pengguna untuk melanjutkan investigasi Anda. Saat Anda menelusuri lalu lintas jaringan, ingatlah urutan serangan dari investigasi Anda sebelumnya. Semoga berhasil!

Tips: 

Gunakan httpfilter di Wireshark untuk hanya melihat lalu lintas HTTP

Anda juga dapat mengklik kanan pada paket mana pun → ikuti Aliran HTTP untuk merekonstruksi permintaan dan respons lengkap antara klien dan server
buka file : pcap traffick di tryhackme  
Anda dapat menggunakan filter Wireshark http.response.code == 302 untuk mencari login yang berhasil. 
gunakan pencarian wire shark :  http.response.code == 302  selanjutnya folow http stream - username=admin&password=astrongpassword123HTTP/1.1 302 Found

<img width="1346" height="658" alt="image" src="https://github.com/user-attachments/assets/f86c5cc0-e190-4ce2-8829-8433b1da7283" />

 
jawaban : astrongpassword123


Apa flag yang ditemukan penyerang di dalam database menggunakan SQLi?


buka file pcap traffick wireshark
klik pencarian : http kita cari sqli di kotak keterangan - cari 192.168.1.9  get account changeusername di kotak wireshark -
copy file as csv - 
"440","2025-08-20 07:38:21.006112","192.168.1.10","192.168.1.9","HTTP","572","GET /account/changeusername.php?q=%25%27+OR+%271%27%3D%271 HTTP/1.1 "
 selanjutnya follow HTTP STRING - 
 LIHAT DETAIL ISI BAGIAN PALING BAWAH -  THM{dumped_the_db}


<img width="1346" height="674" alt="image" src="https://github.com/user-attachments/assets/c541d1b7-2e93-41ae-be68-bcf68520b6e4" />


JAWABAN : THM{dumped_the_db}

# Firewall Aplikasi Web

<img width="1030" height="330" alt="image" src="https://github.com/user-attachments/assets/35dc9014-a0aa-4670-81f1-7c132f891c01" />

Firewall Aplikasi Web (WAF) seringkali menjadi garis pertahanan pertama untuk situs web dan aplikasi web. Sejauh ini di ruangan ini, kita telah fokus pada pendeteksian dan analisis aktivitas berbahaya. Sekarang, kita akan mengalihkan fokus kita ke salah satu alat mitigasi paling efektif yang tersedia. WAF bertindak sebagai penjaga gerbang untuk aplikasi web Anda, memeriksa paket permintaan lengkap, mirip dengan Wireshark tetapi dengan kemampuan untuk mendekripsi lalu lintas TLS dan menyaringnya sebelum mencapai server.

Aturan
WAF (Web Application Firewall) memeriksa dan memutuskan apakah akan mengizinkan permintaan web atau memblokirnya sepenuhnya berdasarkan aturan yang telah ditentukan. Mari kita periksa beberapa kategori aturan firewall .

Jenis Aturan	Keterangan	Contoh Kasus Penggunaan
Blokir pola serangan umum.	Memblokir muatan dan indikator berbahaya yang dikenal.	Blokir User-Agent berbahaya:sqlmap
Tolak sumber berbahaya yang dikenal	Menggunakan reputasi IP, intelijen ancaman, atau pemblokiran geografis untuk menghentikan lalu lintas yang berisiko.	Blokir IP dari kampanye botnet terbaru.
Aturan yang dibuat khusus	Disesuaikan dengan kebutuhan aplikasi spesifik Anda.	Hanya izinkan permintaan GET/POST ke/login
Pembatasan laju dan pencegahan penyalahgunaan	Membatasi frekuensi permintaan untuk mencegah penyalahgunaan	Batasi upaya login hingga 5 kali per menit per IP.
Bayangkan Anda melihat GETpermintaan berulang ke /changeusernamedengan string User-Agent sqlmap/1.9. SQLMap adalah alat otomatis untuk mendeteksi dan mengeksploitasi kerentanan injeksi SQL. Setelah meninjau lalu lintas jaringan, Anda melihat bahwa permintaan tersebut menyertakan payload SQLi.

Anda dapat membuat aturan untuk memblokir pencocokan string User-Agent apa pun sqlmap:

If User-Agent contains "sqlmap"
then BLOCK

Ini adalah contoh sederhana dan WAF modern akan mendeteksi dan memblokir User-Agent mencurigakan yang dikenal secara otomatis. Namun, aturan seperti ini dapat dibuat khusus untuk menyesuaikan aplikasi atau skenario ancaman spesifik Anda, memungkinkan Anda untuk memblokir aktivitas berbahaya tanpa memengaruhi lalu lintas situs normal.

Mekanisme Tantangan-Respons

WAF tidak selalu perlu memblokir permintaan mencurigakan secara langsung. Misalnya, mereka dapat menantang permintaan dengan CAPTCHA untuk memverifikasi apakah permintaan tersebut berasal dari pengguna sungguhan dan bukan bot. Kemampuan ini sangat berharga mengingat lalu lintas bot berbahaya mencapai 37% dari lalu lintas web global. Pendekatan ini bermanfaat untuk aturan firewall dengan peluang lebih tinggi untuk memblokir lalu lintas web yang sah.

<img width="1250" height="481" alt="image" src="https://github.com/user-attachments/assets/ad2d4150-1603-4237-93eb-0b606619aa5f" />

Mengintegrasikan Indikator yang Diketahui dan Intelijen Ancaman
Banyak solusi WAF modern menyertakan kumpulan aturan bawaan yang dirancang untuk mengurangi risiko keamanan OWASP Top 10 , beberapa di antaranya telah kami bahas sebelumnya. WAF juga memanfaatkan umpan intelijen ancaman untuk secara otomatis memblokir permintaan dari alamat IP berbahaya yang dikenal dan User-Agent yang mencurigakan. Mereka menerima pembaruan rutin untuk memerangi ancaman baru dan yang muncul, termasuk dari kelompok APT yang dikenal dan CVE yang baru ditemukan. Lihat bagaimana Cloudflare memelihara daftar IP yang dikurasi, dari sumber seperti botnet, VPN, anonimizer, dan malware, berdasarkan intelijen ancaman global.

Jawablah pertanyaan-pertanyaan di bawah ini.
Apa yang diperiksa dan disaring oleh WAF?

jawaban : Web Requests


Buat aturan firewall khusus untuk memblokir apa pun User-Agentyang cocok dengan "BotTHM".

Anda dapat membuat aturan untuk memblokir pencocokan string User-Agent apa pun sqlmap:

contoh aturan /rules :
If User-Agent contains "sqlmap" di ghanti menjadi  "BotTHM"
then BLOCK

dan hasilnya :  IF User-Agent CONTAINS "BotTHM" THEN block



jawaban : IF User-Agent CONTAINS "BotTHM" THEN block


