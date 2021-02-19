# Openstack PaaS-TA 
   - Openstack 설치
   - Openstack 환경설정
   - bosh 설치
   - PaaS-TA 설치
   
# 사전 환경 설정
   - 컴퓨터 bios상에서 Intel VT-x/EPT or AMD-V/RVI 활성화
   - VMware 생성시 필요 자원 (PaaS-TA까지 설치 시 필요한 최소 자원)
      vcpu : 16core
      ram : 50GB
      hdd : 700GB
   -  네트워크 NAT 2개 할당

# Openstack 설치
   - 패키지 파일 업데이트 및 업그래이드
      sudo apt update
      sudo apt upgrade -y
      sudo apt dist-upgrade -y
      sudo apt-get update
      sudo apt-get upgrade -y

  - github에서 클론하기 위한 git설치
      sudo apt install git -y

  - ifconfig 사용을 위한 넷툴 설치
      sudo apt install net-tools

  - 사용자 생성
      sudo useradd -s /bin/bash -d /opt/stack -m stack
      echo "stack ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/stack
      sudo su - stack

----------------------------------stack 유저----------------------------------
  - devstack 
    git clone https://github.com/openstack-dev/devstack.git
    vi local.conf
    ./stack.sh
  
  - 설치 중 오류발생 시 다음 명령어 입력 후 ./stack.sh
    ./unstack.sh
    ./clean.sh
  
  - Openstack 설치 완료
  
      ![image](https://user-images.githubusercontent.com/58166973/108470675-ad857980-72cd-11eb-8fc9-6c0f0aac29d8.png)
      
   # Openstack 환경설정
      - key pair 생성

  ![image](https://user-images.githubusercontent.com/58166973/108473515-7913bc80-72d1-11eb-84d1-409d367e9c8f.png)
         
      - 보안그룹 생성

   ![image](https://user-images.githubusercontent.com/58166973/108470720-bb3aff00-72cd-11eb-87a6-00ed41098ccf.png)
         
      - 내부 네트워크 생성

   ![image](https://user-images.githubusercontent.com/58166973/108470729-bece8600-72cd-11eb-9c18-a390062ba8c3.png)
         
      - 라우터 생성 후 외부네트워크와 내부 네트워크 연결
     
   ![image](https://user-images.githubusercontent.com/58166973/108470743-c2620d00-72cd-11eb-9158-de07aeadcbe5.png)
      
      - floating ip 생성
      
   ![image](https://user-images.githubusercontent.com/58166973/108470772-cdb53880-72cd-11eb-99df-e1db7d2dedbf.png)
   
      - ubuntu 상에서 라우터 gate way 설정
         netstat -rn (라우팅 테이블 확인)
         sudo route add default gw x.x.x.x
   
   # bosh 설치
      - bosh cli 설치
      
   ![image](https://user-images.githubusercontent.com/58166973/108470786-d3128300-72cd-11eb-9ceb-6a988c0a4740.png)
   
      - 종속성 파일 설치
   
   ![image](https://user-images.githubusercontent.com/58166973/108470793-d7d73700-72cd-11eb-8516-40dc653d262b.png)
   
      - deploy-openstack.sh 파일의 bosh 설치 시 필요한 설정 (

   ![image](https://user-images.githubusercontent.com/58166973/108470831-e1f93580-72cd-11eb-9ea5-801fc6b9c440.png)


















