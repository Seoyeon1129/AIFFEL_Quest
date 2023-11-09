# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김서연.
- 리뷰어 : 임정훈.


# PRT(Peer Review Template)
- [O]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부

> 1. SentencePiece를 이용하여 모델을 만들기까지의 과정이 정상적으로 진행되었는가? 코퍼스 분석, 전처리, SentencePiece 적용, 토크나이저 구현 및 동작이 빠짐없이 진행되었는가? -> O
```
파일에서 문장을 읽고 출력한 부분, 중복제거, 문장 길이를 기준으로 필터링하여 전처리 하였다. unigram과 BPE를 활용해서 모델을 훈련시켰으며 토크나이저가 구현되어 잘 동작되고 있다.
```
> 2. SentencePiece를 통해 만든 Tokenizer가 자연어처리 모델과 결합하여 동작하는가? SentencePiece 토크나이저가 적용된 Text Classifier 모델이 정상적으로 수렴하여 80% 이상의 test accuracy가 확인되었다. -> O
```
확인 결과 모두 80퍼 이상의 성능이 나온다.
```
> 3. SentencePiece의 성능을 다각도로 비교분석하였는가? SentencePiece 토크나이저를 활용했을 때의 성능을 다른 토크나이저 혹은 SentencePiece의 다른 옵션의 경우와 비교하여 분석을 체계적으로 진행하였다. -> O
```python
# Mecab를 사용하였다
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences

def mecab_text_to_seq(data):
    mecab = Mecab()
    
    tokenized_corpus = [' '.join(mecab.morphs(sentence)) for sentence in data]
    
    tokenizer = Tokenizer()
    tokenizer.fit_on_texts(tokenized_corpus)
    
    sequences = tokenizer.texts_to_sequences(tokenized_corpus)
    
    max_len = max(len(sequence) for sequence in sequences)  # 가장 긴 시퀀스 길이 계산
    padded_sequences = pad_sequences(sequences, maxlen=max_len, padding='post')
    
    return padded_sequences
```
    
- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
> 코드에 주석이 거의 달리지 않은 점이 아쉽습니다.
        
- [O]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
> 모델 학습을 10번 돌렸을 때에는 결과가 아주 좋지 않았다. 10번 시도했을 때에는 스케치의 컬러가 많이 반영되어 노이즈가 심했지만, 높은 epoch를 사용하게 되면 유사한 값이 도출될 것 같다. 이러한 과정을 history라는 값에 loss값들을 저장해서 어떤 과정으로 줄어드는지 확인하면 좋을 것 같다. 이번 프로젝트 자체는 기본적으로 구조가 비슷해서 쉬웠지만, 그에 대한 이론은 아직 체화 하지 못한 것 같다. 최근 과제가 학습 시간이 오래 걸리다 보니 미리미리 해야겠다.
        
- [O]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.'
> 전처리 과정에서 텍스트 데이터가 어떤 처리가 된 상태인지, 어떤 형태의 데이터로 존재하는지 파악하는 것이 헷갈린다. 중간 과정마다 데이터 모양을 확인하고 주석으로 기록할 필요가 있겠다.
sentencepiece 토크나이저를 사용했을 때 학습 속도가 현저하게 느렸는데, 패딩 마스크를 적용했기 때문이라고 생각했지만 mecab 토크나이저를 사용할 때에는 속도가 빠르게 학습이 됐고 결과적으로 원인 파악을 하지 못했다.
형태소 분석 측면에서 mecab의 성능이 상대적으로 좋아보였고, 모델 학습 후 성능 지표에서 확실하게 향상된 것을 확인할 수 있었다.
형태소 분석기의 성능을 판단할 수 있는 정량적인 지표는 없을까?
        
- [O]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
```python
   def sp_tokenize(s, corpus): 

    tensor = []

    for sen in corpus:
        tensor.append(s.EncodeAsIds(sen))

    with open("./korean_spm.vocab", 'r') as f:
        vocab = f.readlines()

    word_index = {}
    index_word = {}

    for idx, line in enumerate(vocab):
        word = line.split("\t")[0]

        word_index.update({word:idx})
        index_word.update({idx:word})

    tensor = tf.keras.preprocessing.sequence.pad_sequences(tensor, padding='post')

    return tensor, word_index, index_word

```


# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
 BPE에서 토크나이징 하는 부분이 보이지 않습니다.
