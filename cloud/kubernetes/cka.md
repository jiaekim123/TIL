---
description: https://kubetm.github.io/k8s/91-cka/ready/
---

# CKA 자격증 개요

### 시험 개요 및 접수

* [https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)

#### 가격

* 375불 (쿠폰 사용 시 할인 가능)

#### **쿠폰 정보**

{% embed url="https://scriptcrunch.com/linux-foundation-coupon/" %}

#### 버전 정보: The exam is based on Kubernetes v1.21 (2021.08.29 기준)

#### 시험 시간: 3시간

#### 시험 문항: 24문항

* 난이도에 따라 1\~9점, 모두 실습 형태

#### 합격 기준: 74점 이상

* 시험 완료 후 36시간 후에 이메일로 전

#### 시험 관련 My Portal URL

* 시험 접수 후 확인 가
  * [ https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck](https://www.examslocal.com/ScheduleExam/Home/CompatibilityCheck)
*  체크 리스트 확인
* 특정 센터에서 보는게 아니라 본인의 환경에서 시험을 치를 수 있음.

### 시험 준비

#### 시험 범위

![](<../../.gitbook/assets/image (6).png>)

{% embed url="https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/" %}

#### 오픈북 시험

* 탭 하나를 켜서 쿠버네티스 도큐먼트 접속 가능.\
  – [https://kubernetes.io/docs/](https://kubernetes.io/docs/)\
  – [https://github.com/kubernetes/](https://github.com/kubernetes/)\
  – [https://kubernetes.io/blog/](https://kubernetes.io/blog/)
* 해당 사이트에서 클릭 시 외부로 연결되어 있는 사이트는 사용 불가.

#### 시험준비 관련 URL

시험범위 별로 document가 어디에 있는지 링크를 다 걸어둔 사이트

* **시험 범위 별 docs 링크1** : [https://github.com/leandrocostam/kubernetes-certified-administrator-prep-guide](https://github.com/leandrocostam/kubernetes-certified-administrator-prep-guide)
* **시험 범위 별 docs 링크2** : [https://github.com/walidshaari/Kubernetes-Certified-Administrator](https://github.com/walidshaari/Kubernetes-Certified-Administrator)

#### 시험중

* **레퍼런스 사이트** : 크롬 브라우저를 하나더 열어놓고, 탭 하나만 사용, 사전에 필요한 Url들은 `bookmark`로 정리해 놓으면 좋음
* **메모장** : 시험 툴에서 제공되는 메모장이 있음. 레퍼런스 사이트의 내용을 복붙해서 수정할때 주로 사용했고, 메모장에 있는 내용을 복사할때 드레그가 잘 안됨
* **복붙** : 레버런스 사이트나 메모장에는 Ctrl+C, V가 잘되나 작업 Console에는 해당 명령이 안먹힘, 가이드 문서에 대체 명령이 있으나, 마우스 우클릭 하면 나오는 메뉴에 붙여놓기 기능이 있어서 그것을 사용했음
* **남은시간** : 왼쪽 상단쯤에 timer가 있는데 남은 시간이 표시되는게 아니라 \[======] 이렇게 바 형태로 남은 시간의 양을 알려줌, 그리고 채팅으로 시간 물어보면 정확히 남은 시간을 알려줌
* **주의행동** : 문제 풀다가 무의식적으로 입을 가리거나 문제를 입으로 읽었더니 바로 채팅창으로 주의를 줌. 자세한 주의행동에 대해서는 가이드 문서에 행동규칙 참고
* **작업Cluster** : 기본적으로 Host(node-1)에서 \[kubectl config user-context k8s] 명령을 통해서 원하는 Cluster로 작업 명령을 날림. 시험 문제마다 어떤 클러스터에 연결해서 작업을 하라는 내용이 나와있고, 70%정도가 k8s Cluster에서 작업함. (ik8s 용도는 쿠버네티스 master, node 설치), ssh로 연결시 user권한이고, root 권한이 아니기 때문에 sudo -i 명령을 써서 작업하는 경우가 있는데 작업 후 node-1까지 돌아갈려면 exit를 두번 해야함. (실수로 한번만 해서 worker를 node-1으로 착각할 수 있음)

![](<../../.gitbook/assets/image (13).png>)

* **콘솔** : 단일 콘솔만 사용가능, 멀티 플렉서 Screen인 tmux을 권장하는데 현재 사용하고 있으면 쓰면 좋을 수 있으나, 아니면 굳이 tmux 사용법을 배워서 써야될만큼 멀티 콘솔로 작업을 해야할 필요성은 못 느꼈음.



### 강의

* Udemy 강의
  * [https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/)
  * ** CKA(2021) 1.21 버전**

{% content-ref url="cka.md" %}
[cka.md](cka.md)
{% endcontent-ref %}





#### &#xD;
