+++
date = '2024-12-23T17:39:55+09:00'
draft = false
title = 'WSL(2)에 cuda toolkit 설치하기'
+++

## WSL(2)에 cuda toolkit 설치하기

nvidia 웹사이트에 매번 로그인 하는게 귀찮은 사람들을 위하여
nvidia 홈페이지에 쓰여있는 명령어 가이드라인을 여기에 공유해본다. 2024년 12월 23일 기준이고, 시간이 지남에 따라 cuda version 관련 숫자 - 12.6.3, 12.6.3-1, 12-6 -을 업데이트 해야할 것이다.
혹시나 헷갈려할 사람들을 위해 조금만 더 설명하자면 원 페이지 내용중 apt-get의 경우 구버전 우분투 명령어이기에 임의로 그냥 apt로 수정하였다.

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.6.3/local_installers/cuda-repo-wsl-ubuntu-12-6-local_12.6.3-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-6-local_12.6.3-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-6-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt update
sudo apt -y install cuda-toolkit-12-6
```

PATH, LD_LIBRARY_PATH 환경변수값에 cuda 경로를 추가한다. 원문에는 cuda-12.6 경로를 추가하라고 써있으나 `/usr/local/cuda/bin` 의 심볼릭 링크를 경로에 추가하는것이 더 범용성에서 더 유리하므로 경로를 수정하였다.
```bash
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

`nvcc --version`이 잘 출력된다면 설치완료.
 