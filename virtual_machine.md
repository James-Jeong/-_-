## * Cloud image root password
~~~
https://jaeho.tistory.com/entry/cloud-image-root-password-설정

(Ubuntu)
> apt install libguestfs-tools
> virt-customize -a CentOS-7-x86_64-GenericCloud.qcow2c --root-password password:root.123
~~~
  
## * Cloud init
~~~
- Cloud init sample : https://cloudinit.readthedocs.io/en/latest/topics/examples.html
- OpenStack instance configuration with cloud-init : https://www.admin-magazine.com/Articles/Automated-OpenStack-instance-configuration-with-cloud-init-and-metadata-service/(offset)/3
~~~

## * Install packages
~~~
(CentOS 7)
> virt-customize -a CentOS-7-x86_64-GenericCloud.qcow2c --install wget
> virt-customize -a CentOS-7-x86_64-GenericCloud.qcow2c --install java-1.8.0-openjdk
~~~
