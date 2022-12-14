# π KoChatBART
[**BART**](https://arxiv.org/pdf/1910.13461.pdf)(**B**idirectional and **A**uto-**R**egressive **T**ransformers)λ μλ ₯ νμ€νΈ μΌλΆμ λΈμ΄μ¦λ₯Ό μΆκ°νμ¬ μ΄λ₯Ό λ€μ μλ¬ΈμΌλ‘ λ³΅κ΅¬νλ `autoencoder`μ ννλ‘ νμ΅μ΄ λ©λλ€. νκ΅­μ΄ μ±ν BART(μ΄ν **KoChatBART**) λ λΌλ¬Έμμ μ¬μ©λ `Text Infilling` λΈμ΄μ¦ ν¨μλ₯Ό μ¬μ©νμ¬ μ½ **10GB** μ΄μμ νκ΅­μ΄ λν νμ€νΈμ λν΄μ νμ΅ν νκ΅­μ΄ `encoder-decoder` μΈμ΄ λͺ¨λΈμλλ€. μ΄λ₯Ό ν΅ν΄ λμΆλ λν μμ±μ κ°κ±΄ν `KoChatBART-base`λ₯Ό λ°°ν¬ν©λλ€.

<img src=https://user-images.githubusercontent.com/55969260/205434343-b72641e9-d0f9-4b88-a334-9f904e0a35c5.png>

## Quick tour
```python
from transformers import AutoTokenizer, BartForConditionalGeneration
  
tokenizer = AutoTokenizer.from_pretrained("BM-K/KoChatBART")
model = BartForConditionalGeneration.from_pretrained("BM-K/KoChatBART")

inputs = tokenizer("μλ μΈμμ!", return_tensors="pt")
outputs = model(**inputs)
```

## μ¬μ  νμ΅ λ°μ΄ν° μ μ²λ¦¬
μ¬μ©ν λ°μ΄ν°μ
 - [μ£Όμ λ³ νμ€νΈ μΌμ λν λ°μ΄ν°](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=543)
 - [μμκ³΅μΈ κ³ κ° μ£Όλ¬Έ μ§μ-μλ΅ νμ€νΈ](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=102)
 - [νκ΅­μ΄ SNS](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=114)
 - [λ―Όμ μλ¬΄ μλν μΈκ³΅μ§λ₯ μΈμ΄ λ°μ΄ν°](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=619)

KoChatBARTλ₯Ό νμ΅μν€κΈ° μνμ¬ νκ΅­μ΄ λν λ°μ΄ν°μλ€μ μ μ²λ¦¬ ν ν©μ³ λλμ νκ΅­μ΄ λν λ§λ­μΉλ₯Ό λ§λ€μμ΅λλ€.
1. λ°μ΄ν°μ μ€λ³΅μ μ€μ΄κΈ° μν΄ 'γγγγγγ'μ κ°μ μ€λ³΅λ ννμ΄ 2λ² μ΄μ λ°λ³΅λ  λλ 'γγ'μ κ°μ΄ 2λ²μΌλ‘ λ°κΏ¨μ΅λλ€.
2. λλ¬΄ μ§§μ λ°μ΄ν°λ νμ΅μ λ°©ν΄κ° λ  μ μκΈ° λλ¬Έμ KoBART ν ν¬λμ΄μ  κΈ°μ€ μ μ²΄ ν ν° κΈΈμ΄κ° 3μ λλ λ°μ΄ν°λ§μ μ λ³νμ΅λλ€.
3. κ°λͺμ²λ¦¬λ λ°μ΄ν°λ μ κ±°νμμ΅λλ€.

## Model

| Model         | # of params | vocab size |  Type   | # of layers | # of heads | ffn_dim | hidden_dims |
| ------------- | :---------: | :-----: | :----------: | ---------: | ------: | ----------: | ----------: | 
| `KoChatBART` |    139M     | 50265 | Encoder |           6 |         16 |    3072 |         768 |
|               |            |  | Decoder |           6 |         16 |    3072 |         768 |

## λν μμ± μ±λ₯ μΈ‘μ 
λ€μ μ½λ[(Dialogue Generator)](https://github.com/2unju/KoBART_Dialogue_Generator)λ₯Ό κΈ°λ°μΌλ‘ κ° λͺ¨λΈμ fine-tuning νμμ΅λλ€. λν μμ± μ±λ₯ μΈ‘μ μ μν΄ μΆλ‘  μ ν ν¬λμ΄μ§λμ΄ μμ±λ μλ΅μ λ³΅μν ν, BPE tokenizerλ₯Ό μ¬μ©νμ¬ μ€μ  μλ΅κ³Ό μμ±λ μλ΅ μ¬μ΄μ overlap λ° distinctλ₯Ό μΈ‘μ νμμ΅λλ€.
> **Warning** <br>
> μΌλ°μ μΌλ‘ μ§§μ λν λ°μ΄ν°λ‘ λͺ¨λΈμ μ¬μ νμ΅νμκΈ° λλ¬Έμ κΈ΄ λ¬Έμ₯ μ²λ¦¬κ° μκ΅¬λλ νμ€ν¬(μμ½) λ±μ λν΄μλ μ½ν λͺ¨μ΅μ λ³΄μΌ μ μμ΅λλ€.

### μ€ν κ²°κ³Ό
- [κ°μ± λν λ°μ΄ν°](https://github.com/songys/Chatbot_data)

|Training|Validation|Test|
|:----:|:----:|:----:|
|9,458|1,182|1,183|

| Model                  | Param | BLEU-3 | BLEU-4 | Dist-1 | Dist-2 |
|------------------------|:----:|:----:|:----:|:----:|:----:|
| KoBART    | 124M  | 8.73 | 7.12 | 16.85 | 34.89 |
| KoChatBART    | 139M  | **12.97** | **11.23** | **19.64** | **44.53** |
| KoT5-ETRI    | 324M  | 12.10 | 10.14 | 16.97 | 40.09 |

- [μμκ³΅μΈ λν λ°μ΄ν°](https://github.com/2unju/AIHub_Chitchat_dataset_parser)

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
