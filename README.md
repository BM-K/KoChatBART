# 😎 KoChatBART
[**BART**](https://arxiv.org/pdf/1910.13461.pdf)(**B**idirectional and **A**uto-**R**egressive **T**ransformers)는 입력 텍스트 일부에 노이즈를 추가하여 이를 다시 원문으로 복구하는 `autoencoder`의 형태로 학습이 됩니다. 한국어 채팅 BART(이하 **KoChatBART**) 는 논문에서 사용된 `Text Infilling` 노이즈 함수를 사용하여 약 **10GB** 이상의 한국어 대화 텍스트에 대해서 학습한 한국어 `encoder-decoder` 언어 모델입니다. 이를 통해 도출된 대화 생성에 강건한 `KoChatBART-base`를 배포합니다.

<img src=https://user-images.githubusercontent.com/55969260/205434343-b72641e9-d0f9-4b88-a334-9f904e0a35c5.png>

## Quick tour
```python
from transformers import AutoTokenizer, BartForConditionalGeneration
  
tokenizer = AutoTokenizer.from_pretrained("BM-K/KoChatBART")
model = BartForConditionalGeneration.from_pretrained("BM-K/KoChatBART")

inputs = tokenizer("안녕 세상아!", return_tensors="pt")
outputs = model(**inputs)
```

## 사전 학습 데이터 전처리
사용한 데이터셋
 - [주제별 텍스트 일상 대화 데이터](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=543)
 - [소상공인 고객 주문 질의-응답 텍스트](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=102)
 - [한국어 SNS](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=114)
 - [민원 업무 자동화 인공지능 언어 데이터](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=619)

KoChatBART를 학습시키기 위하여 한국어 대화 데이터셋들을 전처리 후 합쳐 대량의 한국어 대화 말뭉치를 만들었습니다.
1. 데이터의 중복을 줄이기 위해 'ㅋㅋㅋㅋㅋㅋ'와 같은 중복된 표현이 2번 이상 반복될 때는 'ㅋㅋ'와 같이 2번으로 바꿨습니다.
2. 너무 짧은 데이터는 학습에 방해가 될 수 있기 때문에 KoBART 토크나이저 기준 전체 토큰 길이가 3을 넘는 데이터만을 선별했습니다.
3. 가명처리된 데이터는 제거하였습니다.

## Model

| Model         | # of params | vocab size |  Type   | # of layers | # of heads | ffn_dim | hidden_dims |
| ------------- | :---------: | :-----: | :----------: | ---------: | ------: | ----------: | ----------: | 
| `KoChatBART` |    139M     | 50265 | Encoder |           6 |         16 |    3072 |         768 |
|               |            |  | Decoder |           6 |         16 |    3072 |         768 |

## 대화 생성 성능 측정
다음 코드[(Dialogue Generator)](https://github.com/2unju/KoBART_Dialogue_Generator)를 기반으로 각 모델을 fine-tuning 하였습니다. 대화 생성 성능 측정을 위해 추론 시 토크나이징되어 생성된 응답을 복원한 후, BPE tokenizer를 사용하여 실제 응답과 생성된 응답 사이의 overlap 및 distinct를 측정하였습니다.
> **Warning** <br>
> 일반적으로 짧은 대화 데이터로 모델을 사전학습하였기 때문에 긴 문장 처리가 요구되는 태스크(요약) 등에 대해서는 약한 모습을 보입니다.

### 실험 결과
- [감성 대화 데이터](https://github.com/songys/Chatbot_data)

|Training|Validation|Test|
|:----:|:----:|:----:|
|9,458|1,182|1,183|

| Model                  | Param | BLEU-3 | BLEU-4 | Dist-1 | Dist-2 |
|------------------------|:----:|:----:|:----:|:----:|:----:|
| KoBART    | 124M  | 8.73 | 7.12 | 16.85 | 34.89 |
| KoChatBART    | 139M  | **12.97** | **11.23** | **19.64** | **44.53** |
| KoT5-ETRI    | 324M  | 12.10 | 10.14 | 16.97 | 40.09 |

- [소상공인 대화 데이터](https://github.com/2unju/AIHub_Chitchat_dataset_parser)

|Training|Validation|Test|
|:----:|:----:|:----:|
|29,093|1,616|1,616|

| Model                  | Param | BLEU-3 | BLEU-4 | Dist-1 | Dist-2 |
|------------------------|:----:|:----:|:----:|:----:|:----:|
| KoBART    | 124M  | 10.04 | 7.24 | 13.76| 42.09 |
| KoChatBART    | 139M  | **10.11** | **7.26** | **15.12** | **46.08** |
| KoT5-ETRI    | 324M  | 9.45 | 6.66 | 14.50 | 45.46 |

## Contributors
<a href="https://github.com/BM-K/KoChatBART/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=BM-K/KoChatBART" />
</a>

## Reference
- [KoBART](https://github.com/SKT-AI/KoBART)
