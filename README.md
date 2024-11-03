# UNI-D 4th Datathon Team 7
## Image Denoising Task Using Restormer

### 실행 방법
`finetuning.ipynb` 파일만을 사용하여 실행합니다. 필요한 라이브러리는 파일 내 코드 셀에서 import하도록 작성되어 있습니다.
`fine_tuned_PSNRloss_15epoch.pth` 가 훈련된 모델입니다.

- **학습 환경**: Google Colab에서 A100 GPU 사용하여 훈련하였습니다.
- 사전학습된 [**Reformer**](https://openaccess.thecvf.com/content/CVPR2022/papers/Zamir_Restormer_Efficient_Transformer_for_High-Resolution_Image_Restoration_CVPR_2022_paper.pdf) 모델을 파인튜닝하였습니다. Training 시 Reformer의 모델 중 `single_image_defocus_deblurring.pth` 모델을 사용합니다. 해당 논문의 깃허브에서 모델을 다운로드하여 로드한 뒤, 파인튜닝을 진행합니다.

### 코드 구조

1. Import: 필요한 라이브러리를 가져옵니다.
2. Path Setting: 루트 디렉토리를 지정합니다.
3. Hyperparameter Setting:
  ```
  CFG = {
      'IMG_SIZE':224, # tuning 시 (128, 128)로 리사이징
      'EPOCHS':15,
      'LEARNING_RATE':3e-4,
      'BATCH_SIZE':16,
      'SEED':42
  }
  ```
4. Fixed RandomSeed: reproduce를 위해 시드를 고정합니다.
5. Data Pre-Processing: 제공된 데이터로부터 데이터 폴더를 재구성하기 위해 최초로 1번만 실행합니다.
6. CustomDataset: 데이터를 불러오는 클래스입니다.
7. Model Define: 모델 구조를 정의합니다. Pretrained만 실행합니다.
8. Train: `single_image_defocus_deblurring.pth` 모델을 기반으로 다양한 loss function을 적용하여 추가 학습을 진행합니다. loss function은 **PSNR ver1**을 이용합니다.
9. Valid: validation set에 대해 psnr을 계산합니다.
10. Test: test set에 대해 이미지를 복원하고 제출 파일을 만듭니다.
