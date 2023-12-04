# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김서연
- 리뷰어 : 서민성


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 퀘스트 문제 요구조건 등을 지칭
    - 한글 코퍼스를 가공하여 BERT pretrain용 데이터셋을 잘 생성하였다.
        - MLM, NSP task의 특징이 잘 반영된 pretrain용 데이터셋 생성과정이 체계적으로 진행되었다. (o)
    - 구현한 BERT 모델의 학습이 안정적으로 진행됨을 확인하였다.
        - 학습진행 과정 중에 MLM, NSP loss의 안정적인 감소가 확인되었다. (o)
    - 1M짜리 mini BERT 모델의 제작과 학습이 정상적으로 진행되었다.
        - 학습된 모델 및 학습과정의 시각화 내역이 제출되었다. (x)

    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인 (o) 
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
        - pretrain_data 생성하는 코드입니다.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        - ```
          def make_pretrain_data(vocab, in_file, out_file, n_seq, mask_prob=0.15):
            """ pretrain 데이터 생성 """
            def save_pretrain_instances(out_f, doc):
                instances = create_pretrain_instances(vocab, doc, n_seq, mask_prob, vocab_list)
                for instance in instances:
                    out_f.write(json.dumps(instance, ensure_ascii=False))
                    out_f.write("\n")
        
            # 특수문자 7개를 제외한 vocab_list 생성
            vocab_list = []
            for id in range(7, len(vocab)):
                if not vocab.is_unknown(id):        # 생성되는 단어 목록이 unknown인 경우는 제거합니다. 
                    vocab_list.append(vocab.id_to_piece(id))
        
            # line count 확인
            line_cnt = 0
            with open(in_file, "r") as in_f:
                for line in in_f:
                    line_cnt += 1
        
            with open(in_file, "r") as in_f:
                with open(out_file, "w") as out_f:
                    doc = []
                    for line in tqdm(in_f, total=line_cnt):
                        line = line.strip()
                        if line == "":  # line이 빈줄 일 경우 (새로운 단락)
                            if 0 < len(doc):
                                save_pretrain_instances(out_f, doc)
                                doc = []
                        else:  # line이 빈줄이 아닐 경우 tokenize 해서 doc에 저장
                            pieces = vocab.encode_as_pieces(line)    
                            if 0 < len(pieces):
                                doc.append(pieces)
                    if 0 < len(doc):  # 마지막에 처리되지 않은 doc가 있는 경우
                        save_pretrain_instances(out_f, doc)
          ```
        
- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 에러가 난 부분에 대한 기록은 없었습니다.
      

- [ ]  **4. 회고를 잘 작성했나요?**
    - 회고가 작성되어있습니다.

              
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
        - 네 준수하여 작성되었습니다.


# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
