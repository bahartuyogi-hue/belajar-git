# belajar-git
# 📚 Cheat Sheet Instalasi Moodle & Server
Panduan lengkap instalasi Web Server, PHP, Database MariaDB, dan Backup otomatis.
---
## 🛠 1. Instalasi Web Server & PHP (Ubuntu Server)
```bash
sudo apt update
sudo apt install apache2 -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-curl php8.2-zip php8.2-gd php8.2-intl php8.2-mysql php8.2-soap php8.2-xml php8.2-mbstring php8.2-gettext -y
php -v # Pastikan versi 8.2
cd /var/www/html/
chmod -R 777 /var/www/html/
mkdir moodledata
chmod -R 777 /var/www/html/moodledata
wget https://download.moodle.org/download.php/direct/stable501/moodle-latest-501.tgz
rm index.html
tar -xvzf moodle-latest-501.tgz
rm moodle-latest-501.tgz
cp -a moodle/. /var/www/html/
rm -rf moodle/
# Konfigurasi Apache
sudo nano /etc/apache2/sites-available/000-default.conf
# Ubah DocumentRoot /var/www/html/ menjadi /var/www/html/public (jika diperlukan)
sudo service apache2 restart

Langkah Instalasi Database di Ubuntu Dekstop

install mariadb
echo "deb [signed-by=/usr/share/keyrings/mariadb-keyring.gpg] https://mirror.mariadb.org/repo/10.11/ubuntu jammy main" \
| sudo tee /etc/apt/sources.list.d/mariadb.list
sudo apt install mariadb-server mariadb-client mariadb-common -y
sudo systemctl restart mariadb
sudo mariadb-upgrade
mariadb –version
service mariadb restart

buat database
mysql -u root -p
CREATE DATABASE moodle
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'%' IDENTIFIED BY 'passwordkuat';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'%';
FLUSH PRIVILEGES

Cara hubungkan 2 
nano /etc/mysql/mariadb.conf.d/50-server.cnf
cari bagian

Ctrl+x

tekan Y
service mariadb restart

Cara aktfikan ekstensi php
nano /etc/php/8.2/apache2/php.ini
service apache2 restart
Cara check versi mariadb     
Mariadb -v





Cara backup Db di server database/ubuntu desktop

1.cd /home/
2. nano backup.sh

isikan file ini

#!/bin/bash

# Konfigurasi
DB_NAME="moodle2"
DB_USER="root"
BACKUP_DIR="/home"
DATE=$(date +"%Y-%m-%d_%H-%M")

# Nama file backup
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_backup_$DATE.sql"

# Proses backup
mysqldump -u $DB_USER $DB_NAME > $BACKUP_FILE

# Optional: hapus backup lebih dari 7 hari
find $BACKUP_DIR -name "${DB_NAME}_backup_*.sql" -type f -mtime +7 -delete
3. save 
ctrl+x selanjutnya tekan Y

4. chmod +x /home/backup.sh


5. crontab -e

6. paste
penjelasan nomor 7 database di backup setiap pukul 21.20 di folder /home/

7. 20 21 * * * cd /home/ && ./backup.sh


Cara backup dari github

Cd /home/
Apt instalaa git (jika git belum di install)
Git clone https://github.com/alysha10/ukkpaket2.git
Cd ukkpaket2
nano backup.sh (di sesuaikan dengan masing-masing konfigurasi)
chmod +x backup.sh
crontab -e
 paste penjelasan nomor  8 database di backup setiap pukul 21.20 di folder /home/
20 21 * * * cd /home/ukkpaket2/ && ./backup.sh

Backup web konfigurasi apache2

sudo mkdir -p /home/backup
sudo timedatectl set-timezone Asia/Makassar
crontab -e
                   paste bagian bawah (  56 21 * * *  artinya pukul 21 lewat 56 menit)
 56 21 * * * /bin/tar -czvf /home/backup/apache2.tar.gz /etc/apache2 >> /home/backup/cron.log 2>&1



