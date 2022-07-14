# Openstack note
  
## System
<img width="693" alt="스크린샷 2022-07-14 오후 8 50 08" src="https://user-images.githubusercontent.com/37236920/178975874-05e43878-e861-4ffc-9a40-dd5691a5649a.png">
  
<img width="784" alt="스크린샷 2022-07-14 오후 8 49 44" src="https://user-images.githubusercontent.com/37236920/178975796-c92f4d26-5388-4006-b535-80d699f97752.png">
  
~~~
A : Openstack Controller node (Ubuntu train)
B : Openstack Compute node (Redhat7 train) + Jenkins + Yum-repository
~~~
  
## Flow
~~~
[Request] > Jenkins(B) > CI > Controller(A) > CD > Compute(B) > Repo(B)
~~~
  
