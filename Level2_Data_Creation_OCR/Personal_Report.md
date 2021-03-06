# 개인회고_이현진

### **학습 목표 및 과정 + 모델 개선 방안**

**학습 목표**

1. OCR task에 대해 이해할 수 있다.
2. 모델의 성능 향상에 있어서 dataset의 중요도를 알 수 있다.
3. 협업에 있어서 git flow를 사용할 수 있다.

**학습 과정**

1. OCR task에 대해서 알아보며, baseline code들을 살펴본다.
2. model structure은 변경하지 못하므로, `augmentation`, `optimizer`, `learning rate`, `scheduler`, `dataset`과 같이 변경해가며 실험해볼 요소들을에 대해서 계획을 세운다.
3. 실험 및 개선점에 대해서는 git issue를 통해서 팀원들과 공유하며 앞으로의 방향에 대해서 의논한다.

### **시도한 점**

1. 기존에 제공된 dataset은 ICDAR17_Korean 536개의 images를 가지고 있다. 이는 noise가 있어 적은 양으로 학습하기에는 위험이 있다 판단하여 데이터 추가를 결정했다.
    - [ ] ICDAR19 : 10,000 images
    - [ ] ICAR ArT : 5,603 images
2. 글자를 더 잘 인식할 수 있도로 augmentation을 시도해 보았다.  `Blur`, `Noise`, `Simulation`, `Color`, `Geometric` 측면에서 실험을 해볼 수 있다.
    - dataset의 크기가 커서 다양한 실험을 해보지는 못했지만 기존에 적용된 `resize`, `crop`, `rotate`를 적용하였을 때, 가장 좋은 성능을 보였다. 즉, 주로 `Geometric` augmentation에서 효과를 보였다.
3. 최적의 `optimizer` + `scheduler`의 조합에 대해서 실험해보았다.
    

### **배운 점**

1. 데이터만으로도 성능이 많이 바뀔 수 있다는 사실을 경험할 수 있었다. 이를 토대로, 논문에서 성능이 10% 올랐다고 할지라도 데이터는 일관되게 해서 평가했는지? 어떤 부분으로서 성능이 향상되었고, 가능한 부분인지에 대해서 더 심도 있게 생각해볼 수 있을 것이다.

2. 하지만, 데이터가 추가된다고 할지라도 항상 성능이 오르는 것이 아니다. 언어마다 annotation guide가 다를 수 있으며 공공 데이터 혹은 신뢰도가 높은 사이트에서 데이터를 가져온다 할지라도 일관성 있는 annotation을 보장할 수 없다. 또한, 아무리 guide에 따라 정확한 annotation을 했을지라도 본인의 task에서의 annotation guide와 다르다면 다른 dataset이라 보아야 한다. 즉, dataset에서의 annotation의 일관성이 매우 중요하다. 

3. hyperparameter tuning은 가장 마지막에 하는 것이 좋다. hyperparameter tuning으로 인한 성능 향상은 보장되어 있으며 그렇닥 해서 눈에 띄는 정도의 성능 향상을 가져다 주는 것은 아니다. 또한 이는 모델의 구조와 데이터 tuning 실험에 있어서 방해가 되기도 한다. 따라서 모델과 데이터의 tuning을 마치고 실행하자. 다른 의미로 **성능 영끌**하자...

4. 과연 train dataset의 분포와 test dataset의 분포가 같은 것은 반드시 좋을까? 단순히 대회로 생각해보는 것이 아니라 현업에서로 확장시켜 보면 test dataset에 대해서 맞춰서 학습을 하다보면 새로운 dataset에 대해서는 원래의 test datset에서의 성능만큼 나올거라 보장 하지 못할 것 같다. 


### **아쉬운 점 + 앞으로의 방향**

1. hyperparameter tuning에 있어서 직접 하나씩 바꿔가면서 실험한 부분에 있어서 이번 대회처럼 짧은 기간에서는 굉장한 시간 소모의 부담이 크다고 느껴졌다. 다음번에는 `wandb sweep` 기능을 사용하여 시간 단축에 기여할 수 있을 것 같다. 또한 시간적인 부분에 있어서 비교하고자 하는 실험에 대해서 어느정도는 감안하고 iteration을 줄여서 경향성을 보고 걸러내보도록 하자.

2. 데이터가 많을 때에는 프로그래밍 능력이 중요하게 여겨지는 것 같다. Data I/O 부분에서의 수정이 많기 때문이다. 또한 고민 없이 실험을 하다가는 `CPU bottleneck`이 발생학 마련이다. 이럴 경우에는 아무리 좋은 GPU를 사용할지라도 CPU가 따라가지 못하게된다. 따라서 다음번에는 프로그래밍 실력을 늘려 실행 속도도 늘리고 더 의미있는 실험을 해보고자 한다.

3. 데이터 부분에 있어서 합성 데이터를 추가해보지 못한 점이 아쉽다. 합성 데이터를 사용하 시에는 Ground Truth도 함께 생성되기 때문에 데이터 증강/보충에 있어서 큰 효과르 줄 수 있을 것이라 생각된다. 특히 OCR task의 경우에 글자는 사람이 만든 feature이기 때문에 합성 데이터의 효과가 크다고 한다. 다음번의 새로운 task가 주어졌을 때에는, 합성 데이터를 사용하여 학습하는 실험을 해보고자 한다.

4. 실험을 하기 전에 있어서 가설을 세워보자. 또한 결과에 대해서 원인을 고민해보자. 비로 단번에 정답을 말할 수느 없을지라도 본인만의 생각을 해보는 훈련이 필요하다. 훈련이 지속되다보면 어느정도의 감이 생길 것이다.

5. validation dataset 또는 train dataset에 대해서 성능을 측정하는 metric을 너무 단순화 하였다. 우리의 실험은 단 하나의 수치를 가지고 성능을 판단할 수 없다. recall과 precision과 같이 다양한 metric을 함께 살펴보면서 각 metric에 대한 정의와 함께 성능 변화에 대한 적절하 해석을 해보도록 하자.

6. 각 task애 대해서 dataset에서의 의미 있는 분포가 무엇인지 생각해볼 필요가 있는 것 같다. 적절한 분포에 맞출 수 있도록 dataset을 구축해보는 경험이 필요할 것 같다.
