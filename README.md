# Openstack PaaS-TA 
   - Openstack 설치
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
      
   # bosh 설치
   



















