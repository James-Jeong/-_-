# Openstack note
  
## System
<img width="693" alt="스크린샷 2022-07-14 오후 8 50 08" src="https://user-images.githubusercontent.com/37236920/178975874-05e43878-e861-4ffc-9a40-dd5691a5649a.png">
  
<img width="778" alt="스크린샷 2022-07-15 오후 9 22 56" src="https://user-images.githubusercontent.com/37236920/179222127-12875e79-9909-45d4-97b6-eb0efe703f82.png">
  
~~~
A : Openstack Controller node (Ubuntu train)
B : Openstack Compute node (Redhat7 train) + Jenkins + Yum-repository
~~~
  
## Flow
~~~
[Request] > Jenkins(B) > CI > Controller(A) > CD > Compute(B) > VM(B) > Repo(B)
~~~
  
