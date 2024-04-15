 <h1 align="Center">LAPORAN WORKSHOP ADMINISTRASI JARINGAN</h1>


<p align="center">
  <img src="img/Logo_PENS.png" alt="Logo PENS">
</p>

<h4 align="Center">Anggota Kelompok 5</h4>

<p align="center">
Arsyita Devanaya Arianto (3122500008) <br>
</p>

<br>
<h4 align="center">
PROGRAM STUDI VOKASI <br>
D-III TEKNIK INFORMATIKA <br>
DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER 
POLITEKNIK ELEKTRONIKA NEGERI SURABAYA <br> 
2023
</h4> <br><br><hr>

# WEB EMAIL SYSTEM

## Persiapan server
   ![alt text](img/pic1.png)

## NTP Client
1. Lakukan instalasi paket layanan sinkronisasi waktu
   ![alt text](img/pic2.png)
2. Lakukan konfigurasi timezone ke Asia/Jakarta
   ![alt text](img/pic3.png)
3. Lakukan konfigurasi Real Time Clock(RTC)
   ![alt text](img/pic4.png)
4. Aktifkan NTP client untuk sinkronisasi waktu
   ![alt text](img/pic5.png)
5. Menyunting file timesyncd.conf untuk mengarah ke NTP server terdekat untuk mendapatkan waktu delay terpendek
   ![alt text](img/pic6.png)
6. Restart layanan sinkronisasi dan cek status
   ![alt text](img/pic7.png)
   ![alt text](img/pic8.png)
7. Cek kesesuaian tanggal
   ![alt text](img/pic9.png)

## Install Web Server (Apache2 + PHP-FM)
1. Install apache 2
   ```
   sudo apt -y install apache2
   ```
2. Lakukan konfigurasi Apache2
   ```
   sudo nano /etc/apache2/conf-enabled/security.conf
   ```
   ![alt text](img/pic10.png)
    ```
   sudo nano /etc/apache2/mods-enabled/dir.conf
   ```
   ![alt text](img/pic11.png)
   ```
   sudo nano /etc/apache2/apache2.conf
   ```
   ![alt text](img/pic12.png)
   ```
   sudo nano /etc/apache2/sites-enabled/000-default.conf
   ```
   ![alt text](img/pic13.png)
   ```
   systemctl reload apache2
   ```
   ![alt text](img/pic14.png)
3. Test ke web browser
   ![alt text](img/pic15.png)

## Install PHP 8.2
1. Lakukan penginstalan
   ![alt text](img/pic16.png)
2. Mengecek versi php dan test
   ![alt text](img/pic17.png)
   ![alt text](img/pic18.png)

## Install PHP-FM
1. Lakukan penginstalan
   ![alt text](img/pic17.png)
2. Konfifurasi PHP-FM pada file konfigurasi Apache
   ![alt text](img/pic18.png)
3. Test validasi PHP-FM degan membuat file info.php di root document
   ![alt text](img/pic19.png)
4. Lakukan test di browser
   ![alt text](img/pic20.png)

# Database System : MariaDB
1. Lakukan penginstalan
   ![alt text](img/pic21.png)
2. Lakukan konfigurasi
   ![alt text](img/pic22.png)
3. Lakukan restart mariadb
   ![alt text](img/pic23.png)
4. Inisialisasi konfigurasi dan testing database mariadb
   ```
   sudo mysql_secure_installation
   ```
   ![alt text](img/pic24.png)
   ```
   sudo mysql
   ```
   ![alt text](img/pic25.png)
   ```
   show grants for root@localhost;
   ```
   ![alt text](img/pic26.png)
   ```
   select user,host,password from mysql.user; 
   ```
   ![alt text](img/pic27.png)
   ```
   show databases; 
   ```
   ![alt text](img/pic28.png)
   ```
   create database test_database;
   ```
   ![alt text](img/pic29.png)
   ```
   create table test_database.test_table (id int, name varchar(50), address varchar(50), primary key (id)); 
   ```
   ![alt text](img/pic30.png)
   ```
    insert into test_database.test_table(id, name, address) values("001", "Debian", "Hiroshima"); 
   ```
   ![alt text](img/pic31.png)
   ```
    select * from test_database.test_table;
   ```
   ![alt text](img/pic32.png)
   ```
    drop database test_database; 
   ```
   ![alt text](img/pic33.png)

## Melakukan Install PHPMYADMIN
1. Lakukan penginstalan
   ```
    sudo apt install phpmyadmin
   ```
2. Pilih apache2 sebagai web server
   ![alt text](img/pic34.png)
3. Pilih ya untuk konfigurasi database phpmyadmin
   ![alt text](img/pic35.png)
4. Masukkan password
   ![alt text](img/pic37.png)
   ![alt text](img/pic38.png)
5. Create Apache Configuration for phpMyAdmin
   ```
    sudo nano /etc/apache2/apache2.conf
   ```
   ![alt text](img/pic39.png)
6. Membuka phpmyadmin di browser
   ![alt text](img/pic40.png)
   ![alt text](img/pic41.png)
7. Menambahkan prifilage ke user phpmyadmin
   - Login ke mariadb
   - Masukkan perintah GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION; FLUSH PRIVILEGES;
   ![alt text](img/pic42.png)
   ![alt text](img/pic43.png)

## Melakukan install email system
1. Lakukan penginstalan
   ```
    sudo apt -y install postfix sasl2-bin
   ```
2. Pilih no konfigurasi
   ![alt text](img/pic44.png)
3. Buat salinn file konfigurasi default
   ```
    sudo cp /usr/share/postfix/main.cf.dist /etc/postfix/main.cf
   ```
   ![alt text](img/pic45.png)
4. Edit konfigurasi
   ```
    sudo nano /etc/postfix/main.cf
   ```
   ![alt text](img/pic46.png)
5. Perbarui database alias
   ```
    sudo newaliases
   ```
   ![alt text](img/pic.png)
6. Lakukan restart postfix
   ```
    systemctl restart postfix
   ```
   ![alt text](img/pic.png)

## Menambahkan knfigurasi anti spam
1. Lakukan edit file main.cf
   ![alt text](img/pic.png)
2. Restart postfix
   ![alt text](img/pic.png)

## DOVECOT : IMAP4 (TCP 143) and POP3 (TCP110) Server
1. Lakukan instal Devecot Server
   ```
    sudo apt -y install dovecot-core dovecot-pop3d dovecot-imapd
   ```
2. Edit file defecot.conf
   ```
    sudo nano /etc/dovecot/dovecot.conf
   ```
   ![alt text](img/pic.png)
3. Edit file 10-auth.conf
   ```
    sudo nano /etc/dovecot/conf.d/10-auth.conf
   ```
   ![alt text](img/pic.png)
   ![alt text](img/pic.png)
4. Edit file 10-mail.conf
   ```
    sudo nano /etc/dovecot/conf.d/10-mail.conf
   ```
   ![alt text](img/pic.png)
5. Edit file 10-master.conf
   ```
    sudo nano /etc/dovecot/conf.d/10-master.conf
   ```
   ![alt text](img/pic.png)
6. Restart devecot
   ```
    sudo systemctl restart dovecot
   ```

## Final Check semua servis
1. Menampilkan semua koneksi jaringan
   ```
    netstat -a| grep LISTEN
   ```
   ![alt text](img/pic.png)
2. Cek layanan posfix
   ```
    telnet mail.kelompok6.local 25
   ```
   ![alt text](img/pic.png)

## Install webmail (RoundCube)
1. Configure SSL/TLS settings on Apache2 Server, refer to here. 
   
   
2. Buat database untuk RoundCube
   ```
    sudo mysql -u root -p
   ```
   ![alt text](img/pic.png)
3. Install dan konfigurasi RoundCube
   ```
    sudo apt -y install roundcube roundcube-mysql
   ```
   ![alt text](img/pic.png)
   ![alt text](img/pic.png)
4. Berpindah direktori
   ```
    cd /usr/share/dbconfig-common/data/roundcube/install
   ```
5. Jalankan perintah dan masukkan password
   ```
    sudo mysql -u roundcube -D roundcube -p < mysql
   ```
6. Lakukan edit pada file debian-db.php
   ```
    sudo nano /etc/roundcube/debian-db.php
   ```
   ![alt text](img/pic.png)
7. Lakukan edit pada file config.inc.php
   ```
    sudo nano /etc/roundcube/config.inc.php
   ```
   ![alt text](img/pic.png)
   ![alt text](img/pic.png)
8. Lakukan edit pada file roundcube.conf
   ```
    sudo nano /etc/apache2/conf-enabled/roundcube.conf
   ```
   ![alt text](img/pic.png)
9.  Lakukan restart apache2
    ```
    sudo systemctl restart apache2
   ```

## Install Thunderbird (Email GUI Client)
1. Lakukan penginstalan
   ```
    flatpak install flathub org.mozilla.Thunderbird
   ```
2. install thunderbird GUI on https://flathub.org/apps/org.mozilla.Thunderbird
   ![alt text](img/pic.png)
3. Menambahkan 2 email
   ![alt text](img/pic.png)
4. Coba saling sand email
   ![alt text](img/pic.png)
   ![alt text](img/pic.png)
   
   
   