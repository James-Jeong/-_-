# Openstack note
  
## System
~~~
A : Openstack Controller node (Ubuntu train)
B : Openstack Compute node (Redhat7 train) + Jenkins + Yum-repository
~~~
  
## Flow
~~~
[Request] > Jenkins(B) > CI > Controller(A) > CD > Compute(B) > Repo(B)
~~~
  
