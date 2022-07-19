# anomaly-detection-competition

## 파일 경로
```bash
Anomaly Detection Competition
│   README.md
│
└───history
│   │   Add_EfficientNet V2, Parapmeter.ipynb
│   │   Add_Focal loss.ipynb
│   │	Add_Tune.ipynb
│   │	Add_lr scheduler,amp.ipynb
│   │	Add_mixup, silu, dropout, classifier.ipynb
│   │	Add_valid_transforms.ipynb
│   │	Add_xavier, autoaugmentation.ipynb
│   └──	inference_code.ipynb
│  
│
└───models
│   │	eff v2 imageNet radam.pth
│   │	eff v2 sm Adamw 2e-4 2e-2.pth
│   └──	eff3 2e-2 adamw.pth
│
└───src
│   │	ops.py
│   │	autoaugment.py
│   │	Train.ipynb
│   │	Tune.ipynb
│   └──	inference_code.ipynb
│
└───input
    │	test_df.csv
    └──	train_df.csv
```

## 대회 설명
![image](https://user-images.githubusercontent.com/80466735/179725122-58619f8d-3f6d-4045-bb1a-32ce20a87420.png) 
여러 이미지 중에 어떤 class, state가 합쳐져있는 label이 있다. train_df를 보면 어떤 물건인지 상태가 어떤지 알려준다. 예측에서는 상태와 어떤 물건인지 맞추는 대회다. 

[대회 경로](https://dacon.io/competitions/official/235894/overview/description)
[데이터셋](https://dacon.io/competitions/official/235894/data)

## 모델 요약
- [baseline](https://dacon.io/competitions/official/235805/codeshare/3620?page=1&dtype=recent)

- Data Augmentation
    [AutoAugment](https://github.com/DeepVoltaire/AutoAugment)와 flip과 flop을 이용했다. OOM문제로 인해 적절히 사용했다. AutoAugment 중 IMAGENET 데이터셋에 최적화된 augment만 사용했다.
- Model
efficientNet-b3와 efficientNet V2-sm만 사용했다. 기존까지 efficientNet-b3만 사용해왔는데 이번에 V2를 사용했다. 이유는 더 적은 메모리와 학습속도가 빠르다는 내용을 논문에서 접할수 있었다. 실제로 적용해본결과 논문처럼 6.x가 빠르진 않지만 FLOPs와 처리속도가 한 epcoh당 5~10초 빨랐다.

- TTA

[ttach](https://github.com/qubvel/ttach) <br>
Rotate90()<br>
Multiply()

## 사용법
- hyperparameter tuning
튜닝은 메모리가 가능하다면 하는 걸 추천한다.(colab에서는 안돌아감) ray tune을 사용했다.
- 학습시
모든 파일을 clone후 적절한 src/Train.ipynb 파일에 있는 경로를 바꾸면 된다.(데이터셋은 위에 언급된 경로에서 다운)
ops.py와 autoaugment 파일 같은 경우  autoaugment에 필요한 파일이기에 삭제하면 안된다.

- 모델 평가시
src/inference_code.ipynb 파일에서 경로 변경 후 사용한다. 여기에 있는 모델을 사용할 경우 models에 있는 파일을 다운받아 사용하면 된다. 


## 결론
당연한 소리지만 적절히 변수를 변경하며 좀더 무거운 모델을 사용한다면 충분히 80%대는 갈 수 있을 거다. 필자는 비교적 가벼운 모델과 간단한 조절로 private 71퍼에 도달했다.

![image](https://user-images.githubusercontent.com/80466735/179728982-76551f0a-e4ef-4b4e-8547-02ebdacb99b7.png)

이 글을 읽고 바꾸는 분들은 아마도 훨씬 높고 좋은 결과에 달할수 있을 것이다.


