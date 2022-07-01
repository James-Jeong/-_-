## 1) MySQL 설치
~~~
https://jjeongil.tistory.com/1292
~~~
  
## 2) 패키지 레포지토리 활성화
~~~
add-apt-repository cloud-archive:train
~~~
  
## 3) NTP
~~~
https://docs.openstack.org/install-guide/environment-ntp-controller.html
~~~
  
## 4) DB 설정
~~~
https://docs.openstack.org/install-guide/environment-sql-database-ubuntu.html

!!! 꼭 db bind address 확인해야 한다. > 오픈스택 모듈에서 다 붙어야하니까
/etc/mysql/mysql.conf.d/mysqld.cnf > bind-address = 127.0.0.1
~~~
  
## 5) MQ 설정
~~~
https://docs.openstack.org/install-guide/environment-messaging-ubuntu.html

rabbitmqctl add_user openstack openstack.123
rabbitmqctl set_permissions openstack ".*" ".*" ".*"
~~~
  
## 6) Memcached 설정
~~~
https://docs.openstack.org/install-guide/environment-memcached-ubuntu.html
~~~
  
## 7) Keystone 설치
~~~
https://docs.openstack.org/keystone/train/install/keystone-install-ubuntu.html

GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'localhost' \
IDENTIFIED BY 'keystone.123';
GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' \
IDENTIFIED BY 'keystone.123';

keystone-manage bootstrap --bootstrap-password admin.123 \
  --bootstrap-admin-url http://controller:5000/v3/ \
  --bootstrap-internal-url http://controller:5000/v3/ \
  --bootstrap-public-url http://controller:5000/v3/ \
  --bootstrap-region-id RegionOne
~~~
  
## 8) 서비스 프로젝트 추가
~~~
openstack project create --domain default --description "Service Project" service
~~~
  
## 9) Glance 설치
~~~
https://docs.openstack.org/glance/train/install/install-ubuntu.html
~~~
  
## 10) Placement 설치
~~~
https://docs.openstack.org/placement/train/install/install-ubuntu.html

GRANT ALL PRIVILEGES ON placement.* TO 'placement'@'localhost' \
  IDENTIFIED BY 'placement.123';

GRANT ALL PRIVILEGES ON placement.* TO 'placement'@'%' \
  IDENTIFIED BY 'placement.123';
~~~
  
## 11) Controller - nova 설치
~~~
https://docs.openstack.org/nova/train/install/controller-install-ubuntu.html

GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'localhost' \
  IDENTIFIED BY 'nova.123';
GRANT ALL PRIVILEGES ON nova_api.* TO 'nova'@'%' \
  IDENTIFIED BY 'nova.123';

GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'localhost' \
  IDENTIFIED BY 'nova.123';
GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'%' \
  IDENTIFIED BY 'nova.123';

GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'localhost' \
  IDENTIFIED BY 'nova.123';
GRANT ALL PRIVILEGES ON nova_cell0.* TO 'nova'@'%' \
  IDENTIFIED BY 'nova.123';
~~~
  
## 12) Neutron 설치 - controller
~~~
https://docs.openstack.org/neutron/train/install/controller-install-ubuntu.html

GRANT ALL PRIVILEGES ON neutron.* TO 'neutron'@'localhost' \
  IDENTIFIED BY 'neutron.123';

GRANT ALL PRIVILEGES ON neutron.* TO 'neutron'@'%' \
  IDENTIFIED BY 'neutron.123';
~~~
  
### 12-1) Provider network 로 설치
~~~
https://docs.openstack.org/neutron/train/install/controller-install-option1-ubuntu.html
~~~
  
### 12-2) Metadata 설정
~~~
metadata_proxy_shared_secret = metadata.123
~~~
  
## 13) Launch an instance
~~~
https://docs.openstack.org/install-guide/launch-instance.html#create-m1-nano-flavor
~~~
  
### 13-1) Flavor 추가 (VM 시스템 리소스 정보)
~~~
openstack flavor create --id 0 --vcpus 4 --ram 4096 --disk 20 4c_4g_20g
~~~
  
### 13-2) Key pair 추가
  
### 13-3) Security group rules 추가
  
## 14) Network 추가 (provider)
~~~
https://docs.openstack.org/install-guide/launch-instance-networks-provider.html

openstack subnet create --network provider \
  --allocation-pool start=START_IP_ADDRESS,end=END_IP_ADDRESS \
  --dns-nameserver DNS_RESOLVER --gateway PROVIDER_NETWORK_GATEWAY \
  --subnet-range PROVIDER_NETWORK_CIDR provider

openstack subnet create --network provider \
--allocation-pool start=100.100.100.2,end=100.100.100.254 \
--dns-nameserver DNS_RESOLVER --gateway 100.100.100.1 \
--subnet-range 100.100.100.0/24 provider
~~~
  
## 15) Compute 노드에 nova-compute 설치
~~~
https://docs.openstack.org/nova/train/install/compute-install-rdo.html
(B 장비는 Redhat7 > train)

* Compute 노드에 neutron 설치
https://docs.openstack.org/neutron/train/install/compute-install-rdo.html
(설정할 섹션 또는 필드가 없으면 추가하면 된다.)
~~~
  
## 16) Compute 에서 Controller 로 연동
~~~
systemctl start libvirtd.service openstack-nova-compute.service

!!! nova.conf 에 neutron 설정 추가해야한다.
auth_url = http://controller:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = neutron
password = neutron.123
~~~
  
## 17) Controller 에서 확인
~~~
https://docs.openstack.org/nova/train/install/compute-install-rdo.html

root@MEDIDA-OPS-CON:~# openstack compute service list --service nova-compute
+----+--------------+-------------+------+---------+-------+----------------------------+
| ID | Binary       | Host        | Zone | Status  | State | Updated At                 |
+----+--------------+-------------+------+---------+-------+----------------------------+
| 15 | nova-compute | MEDIA-DEV33 | nova | enabled | up    | 2022-06-30T03:19:45.000000 |
+----+--------------+-------------+------+---------+-------+----------------------------+

root@MEDIDA-OPS-CON:~# su -s /bin/sh -c "nova-manage cell_v2 discover_hosts --verbose" nova
Found 2 cell mappings.
Skipping cell0 since it does not contain hosts.
Getting computes from cell 'cell1': c3aa140e-0eac-4168-9673-69b03594f1b4
Checking host mapping for compute host 'MEDIA-DEV33': 72f35c95-319c-4810-926a-809cb1702f0a
Creating host mapping for compute host 'MEDIA-DEV33': 72f35c95-319c-4810-926a-809cb1702f0a
Found 1 unmapped computes in cell: c3aa140e-0eac-4168-9673-69b03594f1b4
~~~
  
## 18) Image
~~~
https://docs.openstack.org/newton/ko_KR/install-guide-rdo/glance-verify.html
~~~
  
### 18-1) Download image & set root password
~~~
1. CentOS cloud image : http://cloud.centos.org/centos/7/images/
2. Set root password
- Install libguestfs-tools (ubuntu : apt install libguestfs-tools)
- virt-customize -a [Image-file-name] --root-password password:[Custom-Password]
~~~
  
### 18-2) Create image
~~~
openstack image create "CentOS-7-x86_64-GenericCloud.qcow2" \
  --file CentOS-7-x86_64-GenericCloud.qcow2 \
  --disk-format qcow2 --container-format bare \
  --public
  
openstack image create "CentOS-7-x86_64-GenericCloud.qcow2c" \
  --file CentOS-7-x86_64-GenericCloud.qcow2c \
  --disk-format qcow2 --container-format bare \
  --public
~~~
  
## 19) Server
~~~
openstack server create \
 --flavor 4c_4g_20g \
 --image CentOS-7-x86_64-GenericCloud.qcow2c \
 --network provider \
 --security-group default --key-name mykey \
 test1

 --user-data script.sh
~~~
  
## 20) Dashboard (horizon) 설치
~~~
https://docs.openstack.org/horizon/train/install/install-ubuntu.html
~~~
  
## 21) 생성된 VM 인스턴스로 접속
~~~
virsh list
virsh instance [INSTANCE-NAME]
~~~

## 22) Local Repository
~~~
https://jjeongil.tistory.com/1292
~~~
  


