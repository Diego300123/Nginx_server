Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  config.vm.hostname = "nginx"
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provision "shell", inline: <<-SHELL
    apt update && apt upgrade -y
    apt install -y nginx git vsftpd

    systemctl enable nginx
    systemctl start nginx

    mkdir -p /var/www/new_web/html
    cd /var/www/new_web/html
    git clone https://github.com/cloudacademy/static-website-example .

    chown -R www-data:www-data /var/www/new_web
    chmod -R 755 /var/www/new_web

    cat <<EOF > /etc/nginx/sites-available/nginx.conf
server {
    listen 80;
    server_name nginx www.nginx;
    root /var/www/new_web;
    index index.html index.php;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    location / {
        try_files \$uri \$uri/ =404;
    }
}
EOF

    ln -sf /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/nginx.conf
    nginx -t
    systemctl reload nginx

    mkdir -p /home/vagrant/ftp
    chown nobody:nogroup /home/vagrant/ftp
    chmod a-w /home/vagrant/ftp

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
      -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/CN=nginx"

    cat <<EOF > /etc/vsftpd.conf
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
listen_ipv6=NO
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
pasv_min_port=40000
pasv_max_port=50000
local_root=/home/vagrant/ftp
EOF

    systemctl restart vsftpd

    echo -e "vagrant\nvagrant" | sudo passwd vagrant
    usermod -d /home/vagrant/ftp vagrant
    chown -R vagrant:vagrant /home/vagrant/ftp
  SHELL

  config.vm.synced_folder ".", "/vagrant", type: "rsync"
end
