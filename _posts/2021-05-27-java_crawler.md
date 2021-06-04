---
layout: single
title:  "사진 합성 AI 기술 "
---
## StarGAN-v2 repository

[https://github.com/clovaai/stargan-v2](https://github.com/clovaai/stargan-v2)

## 로컬환경 사전 요구사항

- git 설치
- 파이썬 설치
- cuda 설치
- PyCharm IDE를 사용하면 편함

**아마 다른 모델을 학습한 경험이 있다면 깔려 있을 것이라고 생각**

만약 깔려 있지 않다면 : sudo apt-get install <설치할 프로그램>

## 코드 설치

```python
# 코드 설치
git clone https://github.com/clovaai/stargan-v2.git

# stargan-v2 디렉터리 이동
cd stargan-v2/

# 필요한 파이썬 환경 설치
# 아나콘다 설치 되어 있어야 함, 해당 환경이 충족되지 않으면 무조건 에러남
conda create -n stargan-v2 python=3.6.7 # 파이썬 3.6.7로 가상환경 생성
conda activate stargan-v2 # 가상환경 활성화
conda install -y pytorch=1.4.0 torchvision=0.5.0 cudatoolkit=10.0 -c pytorch
conda install x264=='1!152.20180717' ffmpeg=4.0.2 -c conda-forge
pip install opencv-python==4.1.2.30 ffmpeg-python==0.2.0 scikit-image==0.16.2
pip install pillow==7.0.0 scipy==1.2.1 tqdm==4.43.0 munch==2.5.0

# wing.ckpt 파일 다운로드
bash download.sh wing

"""
해당 환경을 설치하고 나서 에러 나는 패키지들이 있음
에러 메세지를 보고 
pip install <에러 메세지에서 권장한 패키지명>
으로 다시 설치
"""
# stargan-v2 > expr 디렉터리에 하위에 resume 디렉터리 만들어주기
cd expr
mkdir resume

# 다시 stargan-v2 디렉터리 하위로 이동
cd .. 
```

## 폴더 구조

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa562f99-5530-485a-91f7-9260a943affa/Screen_Shot_2021-05-22_at_9.17.30_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa562f99-5530-485a-91f7-9260a943affa/Screen_Shot_2021-05-22_at_9.17.30_PM.png)

- 다운로드 받은 stargan-v2 코드의 stargan-v2 디렉터리 바로 밑에 네이버 클라우드에 올려둔 취업사진 데이터 폴더를 그대로 갔다 넣는다
- expr/checkpoints/wing.ckpt 파일 꼭 다운 받아졌는지 확인

## 모델 Train 명령어

```bash
# 여자 모델 train
python main.py --mode train --num_domains 3 --w_hpf 1 --batch_size 4 \
               --lambda_reg 1 --lambda_sty 1 --lambda_ds 1 --lambda_cyc 1 \
               --train_img_dir data-female/resume/train \
               --val_img_dir data-female/resume/val
```

```bash
# 남자 모델 train
python main.py --mode train --num_domains 3 --w_hpf 1 --batch_size 4 \
               --lambda_reg 1 --lambda_sty 1 --lambda_ds 1 --lambda_cyc 1 \
               --train_img_dir data-male/resume/train \
               --val_img_dir data-male/resume/val
```

***gpu 메모리 에러 발생했을 경우***

→ batch size를 2로 줄인다

***도메인 갯수 에러 발생 ( num domain 관련)***

```bash
# 도메인 갯수 오류가 날 수도 있음
# data-female/male > resume > train 폴더로 이동

cd data-female/resume/train
ls -a # 숨김파일 보기
rm -rf <숨김파일>

cd data-female/resume/val
ls -a # 숨김파일 보기
rm -rf <숨김파일>

cd data-male/resume/train
ls -a # 숨김파일 보기
rm -rf <숨김파일>

cd data-male/resume/val
ls -a # 숨김파일 보기
rm -rf <숨김파일>
```

***에러 제거 후 다시 train 명령어 실행***
