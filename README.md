# Openstack note
  
## System
![스크린샷 2022-08-29 오전 9 11 31](https://user-images.githubusercontent.com/37236920/187100766-5c2b6de2-7405-4a57-966e-4538538a880b.png)
  
![스크린샷 2022-08-29 오전 9 11 51](https://user-images.githubusercontent.com/37236920/187100775-9ee26002-2a54-4360-9ad1-93aa4d5855d3.png)
  
![스크린샷 2022-08-29 오전 9 12 33](https://user-images.githubusercontent.com/37236920/187100808-069deaac-b23c-4639-9b1a-355ab46dd1e1.png)
  
![스크린샷 2022-08-29 오전 9 13 15](https://user-images.githubusercontent.com/37236920/187100840-440afc15-9368-451e-8f9d-3dbe3de03d29.png)
  
~~~

A : Openstack Controller node (Ubuntu train)
B : Openstack Compute node (Redhat7 train) + Jenkins + Local-Repository(createrepo)
~~~
  
## Flow
~~~
[Request] > Jenkins(B) > CI > Controller(A) > CD > Compute(B) > VM(B) > Repo(B)
~~~
  
