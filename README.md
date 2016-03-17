#Zabbix-Ceilometer Proxy
**HCD ZCP**

##Cách sử dụng

####0. Môi trường yêu cầu

- Một cụm máy chủ cài đặt **OpenStack** phiên bản **Kilo**, có thể tham khảo script tại https://github.com/vietstacker/openstack-kilo-multinode-U14.04-v1 

- Cài đặt Ceilometer trên cụm OpenStack này. Tham khảo hướng dẫn tại http://docs.openstack.org/kilo/install-guide/install/apt/content/ch_ceilometer.html 

- Một máy chủ Zabbix Server phiên bản 2.2. Có thể tham khảo tại https://github.com/hocchudong/ghichep-zabbix


####1. Tại Zabbix host

Log on với quyền root

Cài các gói cần thiết
```sh
apt-get install -y python-pip git
pip install pika
```

Clone repo này về 
```sh
cd /root/
git clone https://github.com/hocchudong/ZabbixCeilometer-Proxy.git
```

####2. Tại Controller node:

*Yêu cầu: Cần cài đặt ceilometer*

Sửa file cấu hình keystone

```vi /etc/keystone/keystone.conf```

```sh 
[DEFAULT]
...
notification_driver = messaging
...
```

####3. Trở lại Zabbix Node

- Tại web của zabbix, download `Template_nova_26-12-14.xml` và import vào Các template Zabbix của bạn (Configuration, Template, Import)

- Khởi chạy chương trình

`cd ZabbixCeilometer-Proxy`

Sửa file proxy.conf và cấu hình các thông số cho đúng với cấu hình zabbix và openstack của bạn. Có thể tham khảo file `proxy.conf` trong github này

Sau khi sửa xong chạy lệnh:

`python proxy.py`

**Chú ý** Bạn có thể tham khảo hướng dẫn tại [youtube](https://www.youtube.com/watch?v=DXz-W9fgvRk)

**Kết quả**

- Xem log của chương trình tại '/var/log/zcp.log'

- Proxy được tạo trên Zabbix: http://prnt.sc/agaj0v

- Các máy ảo được monitor thông qua proxy: http://prnt.sc/agaito

####4. Sử dụng script auto start

```sh
cd /root/ZabbixCeilometer-Proxy
mv proxy /etc/init.d/
chmod 770 /etc/init.d/proxy
/etc/init.d/proxy start
/etc/init.d/proxy stop
update-rc.d proxy defaults
```

##Copyright
Copyright (c) 2014 OneSource Consultoria Informatica, Lda. [🔗](http://www.onesource.pt)

This project has been developed in the scope of the MobileCloud Networking project[🔗](http://mobile-cloud-networking.eu) by Cláudio Marques, David Palma and Luis Cordeiro.

##License
Distributed under the Apache 2 license. See ``LICENSE.txt`` for more information.
