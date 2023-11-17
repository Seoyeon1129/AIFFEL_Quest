# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김서연
- 리뷰어 : 임정훈


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부
        | 평가요소 | 상세기준 |
    > 1. 번역기 모델 학습에 필요한 텍스트 데이터 전처리가 잘 이루어졌다.  데이터 정제, SentencePiece를 활용한 토큰화 및 데이터셋 구축의 과정이 지시대로 진행되었다.
    > 네
```python
ko_tokenizer.encode_as_pieces('작은아버지가 방에 들어가신다.')
['▁작은', '아버', '지가', '▁방에', '▁들어가', '신', '다', '.']
ko_tokenizer.encode_as_ids('작은아버지가 방에 들어가신다.')
[1719, 6209, 641, 12990, 2538, 18821, 18750, 18751]
```
     > 2. Transformer 번역기 모델이 정상적으로 구동된다. Transformer 모델의 학습과 추론 과정이 정상적으로 진행되어, 한-영 번역기능이 정상 동작한다. 
     > 한번씩 번갈아가면서 작동하지만 정상적으로 구동되지는 않습니다.
```
Input: 오바마는 대통령이다.
Predicted translation: it is validation to stay here .
Input: 시민들은 도시 속에 산다.
Predicted translation: i m gladify your resorts .
```
    > 3. 테스트 결과 의미가 통하는 수준의 번역문이 생성되었다. 제시된 문장에 대한 그럴듯한 영어 번역문이 생성되며, 시각화된 Attention Map으로 결과를 뒷받침한다. 
    > 그런 부분은 보이지 않습니다.
    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
    > 네 이해가 됩니다.
```python
# Sentencepiece를 활용하여 학습한 tokenizer를 생성합니다.
import sentencepiece as spm

def generate_tokenizer(corpus, vocab_size, lang="ko",
                       pad_id=0, bos_id=1, eos_id=2, unk_id=3):
    # 말뭉치를 텍스트 파일로 저장합니다.
    temp_file = f'{lang}_corpus.txt'
    with open(temp_file, 'w', encoding='utf-8') as f:
        for line in corpus:
            f.write(f'{line}\n')

    # SentencePiece 모델을 학습하는 데 사용되는 매개변수를 설정합니다.
    spm_args = f"--input={temp_file} --model_prefix={lang}_spm " \
               f"--vocab_size={vocab_size} --pad_id={pad_id} " \
               f"--bos_id={bos_id} --eos_id={eos_id} --unk_id={unk_id} " \
               f"--user_defined_symbols=<SEP>,<CLS>,<MASK> --model_type=bpe"

    # SentencePiece 모델을 학습합니다.
    spm.SentencePieceTrainer.Train(spm_args)

    # 학습된 모델을 로드합니다.
    tokenizer = spm.SentencePieceProcessor()
    tokenizer.Load(f"{lang}_spm.model")

    # 임시 파일을 삭제합니다.
    os.remove(temp_file)

    return tokenizer
```
        
- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
> 네 잘 작성했습니다
> 학습 노드에서 주어진 데이터의 품질이 좋지 않아서 다른 데이터로 대체했다. 기존 데이터의 품질을 확인하는 작업도 노트북에 추가할걸...
        
- [ ]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
> 네 잘 작성했습니다.        
> 학습 노드에서 주어진 데이터의 품질이 좋지 않아서 다른 데이터로 대체했다. 기존 데이터의 품질을 확인하는 작업도 노트북에 추가할걸...
> 토크나이저 학습 단계에서 너무 많은 시간이 소요되고 그럼에도 학습이 이루어지지 않았는데, 작업 중단 후 다시 시도해보니 금방 완료되었다. 시간이 오래 걸리는 단계에서 이것이 원래 시간이 많이 드는 작업인지, 에러에 의한 것인지 확인할 방법이 없을까?
> 번역 품질은 학습이 전부 진행되지 않아서 확실히 이야기할 수는 없으나 그리 좋아 보이진 않는다. 같은 표현이 불필요하게 많이 반복되는 경우도 있고, 그럴 듯한 문장을 출력한 경우에도 원래 한국어 문장과 의미가 유사하지 않은 경우가 많다.
        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
> 네 간결하게 잘 작성했습니다
```python
class LearningRateScheduler(tf.keras.optimizers.schedules.LearningRateSchedule):
    def __init__(self, d_model, warmup_steps=4000):
        super(LearningRateScheduler, self).__init__()
        self.d_model = d_model
        self.warmup_steps = warmup_steps
    
    def __call__(self, step):
        arg1 = step ** -0.5
        arg2 = step * (self.warmup_steps ** -1.5)
        
        return (self.d_model ** -0.5) * tf.math.minimum(arg1, arg2)

learning_rate = LearningRateScheduler(512)
optimizer = tf.keras.optimizers.Adam(learning_rate,
                                     beta_1=0.9,
                                     beta_2=0.98, 
                                     epsilon=1e-9)
```


# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
히트맵을 통해서 모델이 입력의 어떤 부분에 주의를 기울였는지 보면 왜 품질이 낮게 나왔는지 확인할 수 있을 것 같다.
