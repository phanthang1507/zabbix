--------------I. Huong dan backup zabbix-------------
1. bat login user root
3.tao thu muc backupzabbix:
mkdir backupzabbix
2. backup cac thu muc: /zabbix-data /zabbix-mysql /zabbix-nginx: 
dung tu thu muc vua tao:
 tar -czvf BackUpDataZabbix.tar.gz /zabbix-data /zabbix-mysql /zabbix-nginx  #backup 3 file nhu link tuong ung
3. backup thu muc zabbix-docker ( thu muc docker-compose)
dung o thu muc vua tao:
tar -czvf BackUpZabbixDocker.tar.gz /root/zabbix-docker

----------------II. restore zabbix tu file bacup-----------------
1.can copy het cac file nen backup sang may moi can bung file nen ra
mau cau lenh:
scp backup.tar.gz username@remote_server_ip:/path/to/destination
2.lenh giai nen BackUpZabbixDocker.tar.gz #thu muc zabbix-docker:
tar -xzvf BackUpZabbixDocker.tar.gz -C /    # / la duong dan no auto giai nen vao duong dan /root/zabbix-docker
3. lenh giai nen file  BackUpDataZabbix.tar.gz # la cac thu muc /zabbix-data /zabbix-mysql /zabbix-nginx
lenh: 
tar -xzvf BackUpDataZabbix.tar.gz -C /    # giai nen 3 thu muc /zabbix-data /zabbix-mysql /zabbix-nginx

----III. Tien hanh chay docker compose-----------
den thu muc zabbix-docker:
cd /root/zabbix-docker
chay docker compose:
docker-compose up -d
da hoan thanh xong

----IV. Để tự động khởi chạy Docker Compose trong thư mục /root/zabbix-docker mỗi khi khởi động Ubuntu, bạn có thể tạo một systemd service. Dưới đây là các bước để thực hiện điều này------
Sử dụng lệnh sau để tạo hoặc chỉnh sửa tệp này:
sudo nano /etc/systemd/system/zabbix-docker.service
nhap nhu sau:
[Unit]
Description=Zabbix Docker Compose Service
After=docker.service
Requires=docker.service

[Service]
Restart=always
WorkingDirectory=/root/zabbix-docker
ExecStart=/usr/local/bin/docker-compose up -d

[Install]
WantedBy=multi-user.target


=>luu file tren xong.
chay service:
sudo systemctl daemon-reload
sudo systemctl enable zabbix-docker.service
sudo systemctl start zabbix-docker.service
sudo systemctl status zabbix-docker.service
=> khoi dong lai may ok
