## Intro

- PyTorch로 EN-FR 기계번역(영어 → 프랑스어) 모델 구현
- LSTM, LSTM + Attention, Transformer 세 가지 구조 비교 실험
- IWSLT 2017 TED-talk 병렬 코퍼스 사용
- 각 모델의 번역 성능, 학습/검증 Loss, Perplexity, Training Time 등 다양한 지표 분석

## IWSLT 2017 EN-FR 데이터셋

- 영어-프랑스어 병렬 문장 데이터셋
- 총 50,000개 학습 샘플(실험 단축을 위해 subset 사용)
- 실제 TED 강연에서 추출된 자연스러운 문장

## Discussion

- [x] 세 모델의 최종 검증 Loss 차이와 그 원인 분석
- [x] 연산량 및 번역 성능 관점에서 가장 효율적인 모델 논의
- [x] 모델 크기 및 번역 성능 관점에서 가장 효율적인 모델 논의
- [x] 학습 시간 및 번역 성능 관점에서 가장 효율적인 모델 논의

## TODO

- [x] LSTM baseline
- [x] Attention on LSTM - Attention
- [x] Attention on LSTM - AttnDecoder
- [x] Transformer - TransformerModel
- [x] Colab 전체 학습 완료
- [x] 레포트 완성 및 제출