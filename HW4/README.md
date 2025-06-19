## Intro

- PyTorch로 조건부(Conditional) Diffusion Model을 구현하여 CIFAR-10 이미지 생성 실험
- Diffusion Process, Conditional U-Net 백본, 학습 및 샘플링 파이프라인
- 각 클래스별 이미지 생성 및 다양한 시각화(베타/알파 스케줄, 손실 곡선, 생성 이미지)

## CIFAR-10 데이터셋

- 10개 클래스로 구성된 32x32 컬러 이미지 데이터셋
- 총 60,000장 (학습 50,000 + 테스트 10,000)
- class: [ airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck ]

## Discussion

- [x] Diffusion 모델 코드를 읽고, 어떻게 작동하는지 간단히 요약
- [x] 레포트 완성 및 제출