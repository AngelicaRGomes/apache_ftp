# apache_ftp

1 Baixar e Extrair o Source Apache
apt-get update
wget https://downloads.apache.org//httpd/httpd-2.4.46.tar.gz

2 Instalar as bibliotecas apr's
apt-get install libapr1-dev libaprutil1-dev

3 Instalar Compilador 
apt install build-essential

4 Instalar PCRE3
apt-get install libpcre3 libpcre3-dev

5 Instalar Biblioteca expat
apt-get install libexpat1-dev

6 Instalar Biblioteca ssl
apt-get install libssl-dev

7 Instalar o servidor
./configure --enable-ssl --enable-so
Make
Make Install

8 Gerar a chave e certificado
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt

9 Incluir estas linhas no arquivo httpd.conf
Listen "Seu IP":443
<VirtualHost *:443>
    ServerName "Seu IP"
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>



adduser sammy
mkdir /home/sammy/ftp
chown nobody:nogroup /home/sammy/ftp 
chmod a-w /home/sammy/ftp
mkdir /home/sammy/ftp/www
chown sammy:sammy /home/sammy/ftp/www 



12 Alterar arquivo vsftpd.conf

chroot_local_user=YES
user_sub_token=$USER
local_root=/home/$USER/ftp
pasv_min_port=40000
pasv_max_port=50000

13 Criar arquivo de lista de usuario permitido

echo "sammy" | sudo tee -a /etc/vsftpd.userlist


14 Gerar chave fstpd 
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem

# rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
# rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem

ssl_enable=YES (ALTERAR)
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH


15 Bloquear conexão SSH

nano /bin/ftponly >>> 

#!/bin/sh
echo "This account is limited to FTP access only."

chmod a+x /bin/ftponly
sudo nano /etc/shells   >>
/bin/ftponly
usermod sammy -s /bin/ftponly
