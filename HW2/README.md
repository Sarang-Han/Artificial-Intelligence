## Intro

- PyTorch를 사용하여 ResNet 스타일의 CNN 구현
- CIFAR-10 데이터셋을 이용한 간단한 객체 분류 수행
- 다양한 정규화 기법 실험

## CIFAR-10 데이터셋

- 10개의 클래스로 구성된 작은 컬러 이미지 데이터셋
- 총 60,000장 (학습 50,000 + 테스트 10,000)
- class: [ airplane, automobile, bird, cat, deer, dog, frog, hourse, ship, truck ]

## Discussion

- [x] 각 실험에 대한 Loss와 Accuracy 그래프를 보고, 과적합이 발생했는지 여부를 분석하세요.
- [x] 각 정규화 기법의 효과를 비교하세요.
- [x] 정규화 기법들의 하이퍼파라미터를 변경하고, 그 결과를 비교하세요.