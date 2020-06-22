## Deep Learning is Robust to Massive Label Noise
- [논문 정보](https://arxiv.org/abs/1705.10694)

### Abstract & Conclusion
이 논문에서는 deep neural network가 압도적으로 true label < incorrect label 인 training data라도 학습이 잘 될 수 있음을 보여준다. label noise가 많아도 잘 분류된 데이터(clean label data)의 수가 많거나 batch size, learning rate 조정으로 학습이 잘 될 수 있다고!


### label noise 종류에 따른 3가지 실험
Annotation은 data labeling하는 작업을 말하는데, 이 작업이 expensive한 작업(돈, 시간, 노력 측면에서)이기 때문에 딥러닝이 noisy한 데이터에서도 높은 예측 정확도를 보장할 수 있다면 더 많은 데이터가 활용될 수 있다고 말한다.

이미지 분류에 초점을 맞춰서 MNIST, CIFAR-10, ImageNet 데이터를 사용했고, label noise 종류에 따라 3가지 실험을 진행했다.

1. 첫번째 실험은 uniform label noise.

    기존 correct label은 놔두고, uniform 분포를 따르는 label noise를 생성한 케이스. 예를 들면, correct label을 가진 원래 데이터 1개가 있을 때 각 다른 label을 갖는 6개의 데이터를 생성하였다.        
    ![figure1](https://github.com/jsj267/paper-review/blob/master/image/1-f1.JPG?raw=true)![figure2](https://github.com/jsj267/paper-review/blob/master/image/1-f2.JPG?raw=true)![figure3](https://github.com/jsj267/paper-review/blob/master/image/1-f3.JPG?raw=true)          
    세가지 데이터 모두 괜찮은 예측정확도를 보인다. 아무래도 task의 난이도가 높아질수록 label noise의 비율(=α)이 높아지면 정확도가 떨어지긴 하지만 noise비가 100인 상황(α=100)에서 90% 이상의 정확도를 보여주고 있다. 또, MNIST와 CIFAR 실험 결과를 보면 더 큰(논문에서 larger라고 표현되어있는데, 한국말로 어떻게 표현해야할지 모르겠다. 두꺼운? 촘촘한?) 신경망 구조가 noise에 robust하다는 것을 볼 수 있었다. Perceptron < MLP < Conv4 순으로 noise 비율이 높아질수록 정확도가 천천히 떨어진다.

2. 두번째 실험은 Structured label noise.

    실제 상황에서 label noise는 uniform 분포를 따르지 않기 때문에 noise를 bias하게 생성시켜 진행한 케이스. δ=0에 가까울수록 uniform 분포를 따르고, δ=1에 가까울수록 일부 label noise는 수가 많아지고 일부 label noise는 수가 적어진다. 한쪽 noise에 bias가 생기는 상황.     
    ![figure4-1](https://github.com/jsj267/paper-review/blob/master/image/1-f4.JPG?raw=true)        
    아래 그래프는 α=20일 때 MNIST 데이터에서 4-layer ConvNet 신경망을 사용하여 학습한 결과인데, δ이 커질수록 특히 0.6정도 부터는 급격하게 정확성이 낮아짐을 볼 수 있었다. label noise의 배치 순서는 그렇게 큰 차이는 보이지 않는듯. Confusing order(label noise가 많은 순서)가 약간 더 좋은 정확성을 보이기는 한다.      
    ![figure5](https://github.com/jsj267/paper-review/blob/master/image/1-f5.JPG?raw=true)      

3. 세번째 실험은 Source of noisy labels.

    위 두 실험은 label noise를 하나의 dataset에서 사용하였으나,(예를 들면, CIFAR-10 데이터셋만을 training example로 사용했다. 즉, noisy label을 CIFAR-10에서 만들어서 사용.) 현실에서는 해당 데이터셋에 포함되지 않는 카테고리까지 포함할 수 있기 때문에 이를 반영한 세번째 실험. 즉, 이전에는 하나의 소스(데이터셋)만을 사용했다면 세번째 실험에서는 2가지 소스를 이용한다. (1)원래 데이터셋(CIFAR-10) + 비슷하지만 다른 데이터셋(CIFAR-100). (2)원래 데이터셋(CIFAR-10) + white noise.          
    ![figure6](https://github.com/jsj267/paper-review/blob/master/image/1-f6.JPG?raw=true)          
    (1), (2) 모두 이전 실험에서 봤던 기존 CIFAR-10 실험보다 정확성이 높은 것을 볼 수 있다. 아무래도 같은 카테고리를 분류하는 것보다 전혀 다른 카테고리를 분류하는 것이 더 쉽기 때문. CIFAR-100을 사용했던 (1)은 원래 CIFAR-10만 사용했던 실험보다 정확도 하락 속도가 2배 정도 느리고, (2)white noise는 거의 정확성에 영향을 받지 않는 것을 알 수 있다. 학습에 덜 부정적으로 영향을 미친다는 결과!


### larger dataset의 중요성 : Clean label training data의 수가 많을수록 좋다!
1. 아래는 MNIST 데이터를 clean label의 수와 ratio of noisy to clean labels(=α값)를 바꿔가며 ConvNet 신경망을 학습시킨 정확도 그래프이다.
    ![figure8](https://github.com/jsj267/paper-review/blob/master/image/1-f8.JPG?raw=true)          
    위 그래프를 보면 clean label을 가진 데이터의 개수가 일정 이상이 되면 예측 정확도가 급격하게 증가하는 것을 볼 수 있다. (정확도가 낮을 때)모델을 훈련시키는 데 일정이상의 clean data가 필요하고, (어느 정도 훈련이 되어 정확도가 급격하게 증가한 후) clean data가 많아질수록 더 높은 정확성에 도달할 수 있다. 또 α값이 높아질수록 특정 정확도를 만족시키기 위한 clean label의 수가 커짐을 알 수 있다.

2. 같은 실험에서, 아래 그래프는 축을 바꿔서 α값을 x축에 놓고 일정 정확성을 만족시키기 위해 필요한 clean label training set size를 y축으로 놓은 그래프이다.            
    ![figure9](https://github.com/jsj267/paper-review/blob/master/image/1-f9.JPG?raw=true)          
    먼저, α값이 높아질수록 특정 정확성을 얻기 위해 필요한 clean label data 수가 증가하는 것을 볼 수 있다. 그리고 그 증가가 linear하게 이루어지며, α값이 높을수록 더 가파르게 증가한다. 더 높은 정확성을 위해 더 많은 수의 clean label data가 필요한 것도 직관적으로 이해할 수 있다.


### noise label이 있을 때, batch size와 learning rate는 어떤 영향을 미칠까?
1. batch size는 클수록 좋다!

    아래 그래프는 batch size를 달리하며(32~256 + infinite batch size) MNIST데이터에 2-layer ConvNet 신경망을 학습시킨 그래프이다.       
    ![figure10](https://github.com/jsj267/paper-review/blob/master/image/1-f10.JPG?raw=true)        
    batch size가 클수록 noisy label에 더 robust함을 볼 수 있다. 그 이유를 저자는 batch안에서 랜덤하게 섞인 noisy label로부터 update된 gradient는 상쇄되고, correct example로부터 업데이트 된 gradient는 조금씩 합쳐져서(누적된다는 말이 더 좋을 듯) 학습에 기여하기 때문이라고 말한다. (위에 실험들은 batch size를 모두 128로 했다고 했는데 왜 256으로 안했는지 궁금하다.)그래프를 보면 높은 정확도를 계속 유지하고 있는 Hα라는 batch size가 있는데, 이는 infinite batch size, 즉 무한대의 batch size를 뜻한다. (논문에서 이론적으로 infinite batch size를 정의하며 새로운 loss function을 만들어 수식으로 제시하고 있는데, 이해가 어려워 나중에 다시 보기로 하고 결론만 설명하자면...) inifinite batch size에서, α값이 높아져도 정확도가 거의 떨어지지 않음을 보았다. 저자는 noise label의 비율 증가가 true gradient의 방향을 바꾸는 것이라기 보다는 gradient magnitude를 줄인다고 설명한다.

2. learning rate는 작을수록 좋다!

    위에서 batch size는 클수록 학습에 좋다고 하였는데, 이는 noise의 증가가 효과적인 batch size의 폭을 줄인다는 것을 시사한다. batch size가 낮을수록 보다 넓은 폭의 learning rate가 효과적으로 사용될 수 있다(즉, training stability가 좋다)고 알려져 있다. ([참고.](https://blog.lunit.io/2018/08/03/batch-size-in-deep-learning/)) 그렇다면, noise label이 batch size의 범위를 줄이기 때문에, learning rate의 범위 또한 좁아지고, 따라서 더 낮은 learning rate가 noise에 효과적으로 작용할 수 있음을 유추할 수 있다.        
    ![figure11](https://github.com/jsj267/paper-review/blob/master/image/1-f11.JPG?raw=true)        
    그래프로도 확인할 수 있는 내용이다.

batch size는 클수록, learning rate는 작을수록 robust하다고 표현했지만 학습 시간과 같은 trade-off가 존재하므로  '적당히'가 중요하다.


### 마무리하며
논문의 conclusion을 다시 정리해보자면, 1) noisy label의 비율이 커도 clean label 자체의 개수가 많으면 deep learning 은 어느정도 robust하다. 2) 어느 정도의 정확성을 만족시키기 위해 필요한 clean label의 개수는 noisy label의 비율과 linear하게 증가한다. 3) noisy label의 비율이 커질수록 효과적인 batch size, learning rate의 폭이 좁이지기 때문에, 반대로 batch size를 어느정도 크게, learning rate를 어느정도 작게 하는 것은 noise가 있는 학습에 도움이 될 수 있다.

공부하다가 노이즈가 어느 정도의 영향을 끼치는지 궁금해서 찾다가 보게 된 논문인데, 궁금증도 해소하고 재밌었다. 또 데이터의 질에 대해서도 생각해보고. 하지만 데이터의 질을 높인다는 게 얼마나 까다로운 작업일지.. 최적화 과정이 중요하구나 한번 더 깨닫게 되었다.
