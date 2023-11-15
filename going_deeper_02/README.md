# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김서연
- 리뷰어 : 서민성


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 퀘스트 문제 요구조건 등을 지칭
        - 3가지 단어 개수에 대해 8가지 머신러닝 기법을 적용하여 그중 최적의 솔루션을 도출하였다. o
            - ![스크린샷 2023-11-09 오후 5 46 33](https://github.com/Seoyeon1129/AIFFEL_Quest/assets/138687269/43f804f5-061a-486b-b574-04cf18d032b4)
        - Vocabulary size에 따른 각 머신러닝 모델의 성능변화 추이를 살피고, 해당 머신러닝 알고리즘의 특성에 근거해 원인을 분석하였다. o
            - ![스크린샷 2023-11-09 오후 5 47 16](https://github.com/Seoyeon1129/AIFFEL_Quest/assets/138687269/fa6eb6a4-ca6e-4b4e-a469-293cbae90500)
        - 동일한 데이터셋과 전처리 조건으로 딥러닝 모델의 성능과 비교하여 결과에 따른 원인을 분석하였다. o
            - `간단한 LSTM 모델에서 약 63%의 정확도를 보였다.`
            - `1차원 합성곱 신경망 모델에서는 약 73%의 정확도를 보였다.`
      
    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된  주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
        - 네 주석처리가 되어있었습니다. 
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
        - 데이터를 DTM > TF-IDF로 변환하고 이를 6가지 모델로 학습하는 코드였습니다.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        - ![스크린샷 2023-11-09 오후 5 49 36](https://github.com/Seoyeon1129/AIFFEL_Quest/assets/138687269/1d5bc75c-e906-4a8c-bce4-018e9f41fa87)

- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인 
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 실험이 기록되어 있는지 확인
        - 단어의 수에 따라 모델에 따라 도출되는 값에 대한 시각화가 잘 되어있습니다.
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        - ![스크린샷 2023-11-09 오후 5 52 07](https://github.com/Seoyeon1129/AIFFEL_Quest/assets/138687269/f78833fb-2733-47e2-833e-99101dd67569)
        
- [ ]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해 배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
        - 네 작성되어있습니다. 
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
        - 준수하여 작성되었습니다. 
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
        - 함수화 및 모듈화가 가능한 부분은 함수를 만들어서 작성하였습니다. 보고 배울게요.. bb 
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        - 학습에 걸쳐 예측 및 결과값에 획득에 대한 코드가 잘 함수화 되어있습니다.bb
        - ```
          # 각 모델에 대해 학습 및 예측을 함수로 구현
            def fit_and_predict(models, input_train, target_train, input_test):
                res = list()
                for name, model in models:
                    model.fit(input_train, target_train)
                    pred = model.predict(input_test) 
                    res.append((name, pred,)) # 평가 결과를 저장
                    print(f'{name} fit completed')
                return res
          ```
          ```
          # 모델 별로 정확도를 출력하는 함수
            from sklearn.metrics import accuracy_score
            
            def get_accuracy(predictions, target_test):
                df = pd.DataFrame(index=['accuracy'])
                for name, pred in predictions:
                    df[name] = round(100*accuracy_score(target_test, pred), 5)
                return df
          ```
          # 모델별로 F1-스코어 출력하는 함수
            from sklearn.metrics import f1_score
            
            def get_f1_score(predictions, target_test):
                df = pd.DataFrame(index=list(range(num_classes)))
                for name, pred in predictions:
                    df[name] = f1_score(target_test, pred, average=None)
                return df
          ```

# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
