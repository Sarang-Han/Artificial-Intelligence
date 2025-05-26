## 전처리

```py
# 전체 흐름

원시 텍스트 (train.en/fr)
↓
어휘 학습 (SentencePieceTrainer)
↓
텍스트 → ID 변환 (encode())
↓
모델 (LSTM, Transformer)

```

## 1. 말뭉치 수집
- 목표: 영어-프랑스어 병렬 말뭉치를 얻는 것
- `load_dataset()` 함수는 Hugging Face Datasets를 사용해 IWSLT 2017 TED 강연 번역 데이터를 다운로드하고, 각 항목이 사전 형태인 리스트를 반환함
- 두 언어가 나란히 있는 구조는 나중에 시퀀스-투-시퀀스 모델을 훈련할 수 있게 해줌

<br>

## 2. 데이터 샘플링
- 원본 기계 번역 말뭉치는 매우 큼
- 고정된 시드 값(42)을 사용해 데이터를 셔플하고, 그 중 앞부분 `subset_size`만 사용하면:
    - 실험을 재현 가능하게 만들고
    - Colab GPU (예: T4)에서의 훈련 시간을 합리적인 수준으로 유지할 수 있음

<br>

## 3. 원시 텍스트 파일 저장 (train.en, train.fr)
- SentencePiece의 트레이너는 한 줄에 하나의 문장이 있는 평문 텍스트를 입력으로 요구함
- 샘플을 저장하는 이유는 두 가지:
	1. SentencePiece가 요구하는 입력 형식 제공
	2. 모델이 실제로 보게 될 문장들을 텍스트 편집기에서 직접 열어볼 수 있음

<br>

## 4. SentencePiece로 서브워드 어휘 학습 (BPE 사용)
- 어휘 학습이란?
- 딥러닝 모델은 문자열이 아닌 숫자만 처리 가능함
- “어휘 학습”이란 어떤 텍스트 조각을 토큰으로 분리할지 결정하고, 각각의 조각에 고유한 정수 ID를 할당하는 과정임
- 고전적인 NLP에서는 고정된 단어 목록을 사용했고, 희귀하거나 철자가 틀린 단어는 전부 `<unk>` 토큰 하나로 처리 → 정보 손실
    - `<unk>`: 어휘에 없는 어떤 문자 조합이든 이걸로 처리됨
- 요즘은 서브워드 단위를 선호함 (예: BPE, WordPiece, Unigram) → 처음 보는 단어도 유의미한 조각으로 쪼갤 수 있음

<br>

## 5. 런타임 토크나이저 설정
- `SentencePieceProcessor`는 학습된 `bpe.model`을 로드하고 다음 기능들을 제공함:
- `encode(str)` → `List[int]` (문장을 정수 ID 시퀀스로 변환)
- 특수 토큰 ID (예: `pad_id()`, `bos_id()` 등)
- 전체 어휘 크기 (`get_piece_size()`)
- `encode()` 함수는 긴 문장을 `max_len - 1` 토큰으로 잘라내고 명시적으로 `EOS_ID`를 추가함
- RNN이나 Transformer는 언제 디코딩을 멈춰야 할지 아는 게 중요함

<br>

## 6. TranslationDataset
- PyTorch의 Dataset 클래스를 상속
- 원시 문자열을 그대로 가지고 있음 (tokenization을 미룸)
- 토크나이즈는 collate 단계에서 처리함 → 미니배치마다 길이에 맞춰 패딩할 수 있음
- 전체 말뭉치 기준의 최대 길이에 맞춰 패딩하는 것보다 훨씬 메모리 효율적임

<br>

## 7. 미니배치 구성 (collation)
- `collate()` 함수:
    - (src, tgt) 쌍을 `encode()`로 토크나이즈함
    - 배치 내 가장 긴 시퀀스 길이를 찾음
    - 짧은 시퀀스는 오른쪽에 `<pad>` 토큰을 추가해서 `torch.tensor()`로 합칠 수 있는 직사각형 형태(`batch_size` × `seq_len`)로 만듦
- 결과:
    - 두 개의 LongTensor (정수형 텐서)가 생성됨
    - 바로 `nn.Embedding` → 인코더/디코더 → 손실 계산에 사용할 수 있음

<br>

## 8. DataLoader 설정
- 훈련용 DataLoader
- 문장 쌍 50,000개 사용
- 에폭마다 셔플
- 우리가 만든 커스텀 패딩 방식 적용
- 검증용 DataLoader
- 고정된 1,000개 문장 사용
- 순서는 항상 일정함 (deterministic)
- 이 두 DataLoader는 GPU에서 바로 사용할 수 있는 배치를 실시간으로 제공함
→ LSTM이나 Transformer 모델에 곧바로 투입 가능

<br>

---

## SentencePieceTrainer란?

- `SentencePieceTrainer`는 문장 → 숫자 토큰 시퀀스로 바꿔주는 사전을 자동으로 만들어주는 도구
- 딥러닝 자연어처리에서 필수적인 전처리 단계

### 어떻게 작동하는가?

1. 입력
    - 영어와 프랑스어 문장이 각각 한 줄씩 들어있는 텍스트 파일을 줍니다.
2. 서브워드 분할
    - 단어 단위가 아니라, 더 작은 조각(서브워드) 단위로 쪼갭니다.
    - 예시:
        - internationalization → international + ization
        - 처음 보는 단어도 조각으로 쪼개서 어느 정도 의미를 파악할 수 있게 함
3. 토큰 사전 만들기
    - 자주 나오는 조각(서브워드)들을 뽑아서 각각에 **고유한 번호(ID)**를 붙입니다.
    - 예를 들어,
        - "in" → 5
        - "ter" → 23
        - "##ation" → 102
        - 이런 식으로 번호를 붙임
4. 특수 토큰 지정
    - `<pad>`, `<unk>`, `<s>`, `</s>` 같은 특별한 토큰(패딩, 모르는 단어, 문장 시작/끝)도 반드시 고정된 번호로 지정해줍니다.
5. 결과물
    - 이렇게 만들어진 `bpe.model` 파일이 나중에 문장을 숫자 토큰 시퀀스로 바꿔주는 데 사용됩니다.

### 사용 예시

```python
import sentencepiece as spm

spm.SentencePieceTrainer.Train(
    input='train.en,train.fr',   # 입력 파일(영어, 프랑스어)
    model_prefix='bpe',          # 결과 파일 이름(bpe.model, bpe.vocab)
    vocab_size=4000,             # 만들 토큰(조각) 개수
    pad_id=0, pad_piece='<pad>', # 패딩 토큰과 번호
    unk_id=1, unk_piece='<unk>', # 모르는 단어 토큰과 번호
    bos_id=2, bos_piece='<s>',   # 문장 시작 토큰과 번호
    eos_id=3, eos_piece='</s>',  # 문장 끝 토큰과 번호
    model_type='bpe'             # BPE 방식 사용
)
```

<br>

## SentencePieceProcessor란?

- 앞서 만든 `bpe.model` (토큰 사전)을 실제로 사용(번역)할 때 쓰는 도구
- 문장(글자)를 숫자 토큰(정수 ID)로 바꿔주는 역할

**훈련 단계**:
- `SentencePieceTrainer` → `bpe.model`(사전) 생성

**실전/추론 단계**:
- `SentencePieceProcessor`로 bpe.model을 참고하여 입력 문장 → 숫자 토큰(ID) 시퀀스로 변환

### 주요 기능

- `encode(str)`
→ 문장(예: "I love AI")을 숫자 ID 리스트(예: [12, 45, 78, 3])로 바꿈

- 특수 토큰 ID
→ `<pad>`, `<unk>`, `<s>`, `</s>` 같은 특별한 토큰들의 번호(ID)를 알려줌

- 전체 어휘 크기
→ 사전에 등록된 토큰(조각)의 총 개수를 알려줌

## (src, tgt) 쌍을 encode()로 토크나이즈한다는 뜻?

- src: source(원문, 여기선 영어 문장)
- tgt: target(목표, 여기선 프랑스어 문장)

=> 각 배치마다 (en, fr) 문장 쌍을 `encode()` 함수에 넣어서 숫자 토큰 시퀀스로 바꾼다는 뜻

예시:
- "I am a student." → [12, 45, 78, 3]
- "Je suis étudiant." → [21, 33, 99, 3]