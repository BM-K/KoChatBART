# 😎 KoChatBART
[**BART**](https://arxiv.org/pdf/1910.13461.pdf)(**B**idirectional and **A**uto-**R**egressive **T**ransformers)는 입력 텍스트 일부에 노이즈를 추가하여 이를 다시 원문으로 복구하는 `autoencoder`의 형태로 학습이 됩니다. 한국어 채팅 BART(이하 **KoChatBART**) 는 논문에서 사용된 `Text Infilling` 노이즈 함수를 사용하여 **xxGB** 이상의 한국어 대화 텍스트에 대해서 학습한 한국어 `encoder-decoder` 언어 모델입니다. 이를 통해 도출된 대화 생성에 강건한 `KoChatBART-base`를 배포합니다.

<img src=https://user-images.githubusercontent.com/55969260/205434343-b72641e9-d0f9-4b88-a334-9f904e0a35c5.png>

# Performance
## WiseKB 데이터
KHU NLP LAB에서 직접 구축한 양질의 대화 데이터

## 감성 대화 데이터
[일상 대화 데이터](https://github.com/songys/Chatbot_data)

## Reference
- [KoBART](https://github.com/SKT-AI/KoBART)
