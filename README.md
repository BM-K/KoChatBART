# 😎 KoChatBART
[**BART**](https://arxiv.org/pdf/1910.13461.pdf)(**B**idirectional and **A**uto-**R**egressive **T**ransformers)는 입력 텍스트 일부에 노이즈를 추가하여 이를 다시 원문으로 복구하는 `autoencoder`의 형태로 학습이 됩니다. 한국어 채팅 BART(이하 **KoChatBART**) 는 논문에서 사용된 `Text Infilling` 노이즈 함수를 사용하여 **xxGB** 이상의 한국어 대화 텍스트에 대해서 학습한 한국어 `encoder-decoder` 언어 모델입니다. 이를 통해 도출된 대화 생성에 강건한 `KoChatBART-base`를 배포합니다.

<img src=https://user-images.githubusercontent.com/55969260/205434343-b72641e9-d0f9-4b88-a334-9f904e0a35c5.png>

## 사전 학습 데이터 전처리
To be

## Model

| Model         | # of params | vocab size |  Type   | # of layers | # of heads | ffn_dim | hidden_dims |
| ------------- | :---------: | :-----: | :----------: | ---------: | ------: | ----------: | ----------: | 
| `KoChatBART` |    139M     | 50265 | Encoder |           6 |         16 |    3072 |         768 |
|               |            |  | Decoder |           6 |         16 |    3072 |         768 |

## 대화 생성 성능 측정
다음 코드[(Dialogue Generator)](https://github.com/2unju/KoBART_Dialogue_Generator)를 기반으로 각 모델을 fine-tuning 하였습니다. 대화 생성 성능 측정을 위해 추론 시 토크나이징되어 생성된 응답을 복원한 후, BPE tokenizer를 사용하여 실제 응답과 생성된 응답 사이의 overlap 및 distinct를 측정하였습니다.

### 실험 결과
- [감성 대화 데이터](https://github.com/songys/Chatbot_data)

|Training|Validation|Test|
|:----:|:----:|:----:|
|9,458|1,182|1,183|

| Model                  | Param | BLEU-1 | BLEU-2 | BLEU-3 | BLEU-4 | Dist-1 | Dist-2 |
|------------------------|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| KoBART    | 124M  | 19.58 | 12.25 | 9.12 | 7.43 | 14.66 | 31.33 |
| KoChatBART    | 139M  |  |  |  |  |  |  |
| KoT5    | 324M  | 22.61 | 15.46 | 12.10 | 10.14 | 16.97 | 40.09 |

- 소상공인 대화 데이터

|Training|Validation|Test|
|:----:|:----:|:----:|
|29,093|1,616|1,616|

| Model                  | Param | BLEU-1 | BLEU-2 | BLEU-3 | BLEU-4 | Dist-1 | Dist-2 |
|------------------------|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| KoBART    | 124M  | 21.32 | 14.78 | 10.51 | 7.66 | 14.13 | 40.87 |
| KoChatBART    | 139M  |  |  |  |  |  |  |
| KoT5    | 324M  |  |  |  |  |  |  |

## Contributors
<a href="https://github.com/BM-K/KoChatBART/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=BM-K/KoChatBART" />
</a>

## Reference
- [KoBART](https://github.com/SKT-AI/KoBART)
