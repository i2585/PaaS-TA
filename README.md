# Openstack PaaS-TA 
   - [Openstack 설치](#-openstack-설치)
   - [Openstack 환경설정](#-openstack-환경설정)
   - [bosh 설치](#bosh-설치)
   - [PaaS-TA 설치](#PaaS-TA-설치)
   - [PaaS-TA 까지 설치 시 네트워크 구성도 및 필요 리소스](#PaaS-TA-까지-설치-시-네트워크-구성도-및-필요-리소스)
   - [발생 한 오류 및 해결](#발생-한-오류-및-해결)
   
# 사전 환경 설정
   - 컴퓨터 bios상에서 Intel VT-x/EPT or AMD-V/RVI 활성화
   - VMware 생성시 필요 자원 (PaaS-TA까지 설치 시 필요한 최소 자원)
      - vcpu : 16core
      - ram : 50GB
      - hdd : 700GB
   -  네트워크 NAT 2개 할당

# Openstack 설치
   - 패키지 파일 업데이트 및 업그래이드
     - sudo apt update
     - sudo apt upgrade -y
     - sudo apt dist-upgrade -y
     - sudo apt-get update
     - sudo apt-get upgrade -y

  - github에서 클론하기 위한 git설치
     - sudo apt install git -y

  - ifconfig 사용을 위한 넷툴 설치
     - sudo apt install net-tools

  - 사용자 생성
     - sudo useradd -s /bin/bash -d /opt/stack -m stack
     - echo "stack ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/stack
     - sudo su - stack

----------------------------------stack 유저----------------------------------
  - devstack 
     - git clone https://github.com/openstack-dev/devstack.git
     - [vi local.conf](https://github.com/i2585/PaaS-TA/blob/main/devstack%20%EC%84%A4%EC%B9%98/local.conf%20%ED%8C%8C%EC%9D%BC.txt)
     - ./stack.sh
  
  - 설치 중 오류발생 시 다음 명령어 입력 후 ./stack.sh
    - ./unstack.sh
    - ./clean.sh
  
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
      
       ![image](https://user-images.githubusercontent.com/58166973/108470755-c68e2a80-72cd-11eb-9344-9aab49f73769.png)
   
   - ubuntu 상에서 라우터 gate way 설정
       - netstat -rn (라우팅 테이블 확인)
       - sudo route add default gw x.x.x.x
      
# bosh 설치
   - bosh cli 설치
      
      ![image](https://user-images.githubusercontent.com/58166973/108470772-cdb53880-72cd-11eb-99df-e1db7d2dedbf.png)
   
   - 종속성 파일 설치
   
      ![image](https://user-images.githubusercontent.com/58166973/108470777-cf7efc00-72cd-11eb-8975-8cb748b3af81.png)
   
   - deploy-openstack.sh 파일의 bosh 설치 시 필요한 설정 (Openstack 설정에 맞게 적재)

      ![image](https://user-images.githubusercontent.com/58166973/108470786-d3128300-72cd-11eb-9ceb-6a988c0a4740.png)

   - bosh 설치 및 완료
      - ./deploy-openstack.sh
      
      - 설치 완료시 openstack에 생성되는 인스턴스
      
      ![image](https://user-images.githubusercontent.com/58166973/108470842-e6bde980-72cd-11eb-9bd9-f94239f778fb.png)
      
   - bosh login
      - Alias 설정
      
      ![image](https://user-images.githubusercontent.com/58166973/108470849-e9204380-72cd-11eb-9bc2-affb1935aaef.png)
      
      - bosh login shell 스크립트 생성
        - login.sh
        
         ![image](https://user-images.githubusercontent.com/58166973/108470868-ef162480-72cd-11eb-8c6b-c233a2da8907.png)

      - bosh login

       ![image](https://user-images.githubusercontent.com/58166973/108470870-f0475180-72cd-11eb-982f-45507adfc2a8.png)

      - bosh login 확인
      
       ![image](https://user-images.githubusercontent.com/58166973/108470880-f3dad880-72cd-11eb-9255-74d914a1a0ac.png)
      
      - jumpbox key 생성

       ![image](https://user-images.githubusercontent.com/58166973/108470886-f5a49c00-72cd-11eb-8d97-bcc8b0fb1ca5.png)
       ![image](https://user-images.githubusercontent.com/58166973/108470888-f76e5f80-72cd-11eb-8309-bef26e2c47a2.png)
      
      - credhub cli 설치
         - cd
         - wget https://github.com/cloudfoundry-incubator/credhub-cli/releases/download/2.0.0/credhub-linux-2.0.0.tgz
         - tar -xvf credhub-linux-2.0.0.tgz
         - chmod +x credhub 
         - sudo mv credhub /usr/local/bin/credhub 
         - credhub --version
       
      - credhub shell 생성
         - credhub_login.sh 
         
            ![image](https://user-images.githubusercontent.com/58166973/108470903-fd644080-72cd-11eb-8133-08117f7a8f05.png)
            
      - credhub login

       ![image](https://user-images.githubusercontent.com/58166973/108470907-ff2e0400-72cd-11eb-88db-2812f35d5075.png)
      
# PaaS-TA 설치

   - cloud-config 파일 설정 
     - vi openstack-cloud-config.yml
     - ===========================================
     - as-is
      - vm_type:
      -  name: minimal
      -  cloud_properties:
      -  instance_type: m1.tiny
     - to-be - 컴퓨터 사양에 따라 ds1G 이상 사용
      - vm_type:
      - name: minimal
      -  cloud_properties:
      -  instance_type: ds1G
     - ===========================================
     - bosh -e micro-bosh update-cloud-config openstack-cloud-config.yml (업데이트)

   - runtime-config 등록
      - cd ~/workspace/paasta-5.0/deployment/bosh-deployment
      - ./update-runtime-config.sh

   - cloud-config, runtime-config 업데이트 확인
   
      ![image](https://user-images.githubusercontent.com/58166973/108470930-08b76c00-72ce-11eb-845d-bf8f2b00af98.png)

   - stemcell 등록
      - cd ~/workspace/paasta-5.0/stemcell/paasta
      - bosh -e micro-bosh upload-stemcell bosh-stemcell-315.64-openstack-kvm-ubuntu-xenial-go_agent.tgz
    
   - stemcell 등록 확인
   
     ![image](https://user-images.githubusercontent.com/58166973/108470951-10771080-72ce-11eb-8675-7819960e4c9e.png)

   - cloud-config/openstack-cloud-config.yml 파일 내에 설정
	   - 각 가용존에 대해 subnet, gateway, dns, net id를 openstack에 설정한 네트워크에 맞게 변경
	 
   - PaaS-TA 배포
      - cd ~/workspace/paasta-5.0/deployment/paasta-deployment
      - vi deploy-openstack.sh
      		
	  ![image](https://user-images.githubusercontent.com/58166973/108470969-1a007880-72ce-11eb-8264-cf3cecb44548.png)

   - PaaS-TA 설치 완료
   
      ![image](https://user-images.githubusercontent.com/58166973/108471039-313f6600-72ce-11eb-9b0a-a9eee387357a.png)	
      
   - PaaS-TA 설치 확인

      ![image](https://user-images.githubusercontent.com/58166973/108471049-34d2ed00-72ce-11eb-99f8-9c31b1a30379.png)
      
# PaaS-TA 까지 설치 시 네트워크 구성도 및 필요 리소스
   - <네트워크 구성도>
       
       ![image](https://user-images.githubusercontent.com/58166973/108471069-3997a100-72ce-11eb-88be-503ed13f4296.png)
       ![image](https://user-images.githubusercontent.com/58166973/108471084-3e5c5500-72ce-11eb-8cbf-e3f91fa11bca.png)
       
   - <필요 리소스 양>
   
      ![image](https://user-images.githubusercontent.com/58166973/108471095-41efdc00-72ce-11eb-8871-3f489c3879fa.png)
       
# 발생 한 오류 및 해결
   - devstack 설치 시 발생하는 오류 및 해결방안
     - 퍼미션 오류
       - sudo chown -R stack:stack /opt/stack
      
     - simplejson 오류
       - sudo apt purge -y python3-simplejson
     
     - XDG_SESSION_TYPE 오류
       - export XDG_SESSION_TYPE=wayland

   - bosh 설치 시 오류 및 해결
      -	deploy-openstack.sh 파일내의 net id 가 달라 발생하는 오류
      
           ![image](https://user-images.githubusercontent.com/58166973/108470793-d7d73700-72cd-11eb-8516-40dc653d262b.png)
           - 해결: net id의 경우 public이 아닌 생성한 네트워크 subnet의 net id로 설정

      -	리소스 자원 부족으로 인한 오류
      
           ![image](https://user-images.githubusercontent.com/58166973/108470831-e1f93580-72cd-11eb-9ea5-801fc6b9c440.png)
           - 해결: VMware Vcpu, ram 등 리소스 자원 재할당

   - PaaS-TA 설치 시 오류 및 해결
      -	cloud-config/openstack-cloud-config.yml 파일 내의 네트워크 가용존 subnet 오류
      
           ![image](https://user-images.githubusercontent.com/58166973/108479859-98164c80-72d9-11eb-9f6a-3a7d3fd327b8.png)
           - 해결: 파일 내의 가용존 subnet 범위를 다 다르게 적용해서 해결

      - html 400 오류
      
           ![image](https://user-images.githubusercontent.com/58166973/108471008-25ec3a80-72ce-11eb-9c25-6a70d0e7b671.png)
           - 해결: bosh 로그인 확인 또는 재로그인 시 해결
      
      - openstack 내부 네트워크에 subnet이 할당되지 않아 발생한 오류
      
           ![image](https://user-images.githubusercontent.com/58166973/108471020-2a185800-72ce-11eb-942a-0eaac21e5b5f.png)
           - 해결: openstack 내부 네트워크에 subnet 설정 후 라우터 인터페이스 연결

      - vm 생성시 이미지 사이즈 보다 설정된 flavor 하드 용량이 작을 경우
      
           ![image](https://user-images.githubusercontent.com/58166973/108471030-2d134880-72ce-11eb-9d21-6a031fc74609.png)
           - 해결: cloud-config 파일 instance_type 설정 값을 ds1G 변경
