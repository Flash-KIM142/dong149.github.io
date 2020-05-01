---
title: Tensorflow 가이드
layout: post
part: developing
---

Tensorflow 가이드

# Tensorflow Guide

+++

https://www.tensorflow.org/tutorials/keras/classification?hl=ko



## Numpy

python 을 통해 데이터 분석을 할 때 기초 라이브러리로 사용된다.

Numpy 는 C언어로 구현된 파이썬 라이브러리로써, 고성능의 수치계산을 위해 제작되었다. Numerical Python 의 줄임말이기도 한 Numpy 는 벡터 및 행렬 연산에 있어서 매우 편리한 기능을 제공한다.

기본적으로 array라는 단위로 데이터를 관리하며 이에 대해 연산을 수행한다. 

```python
import numpy as np
```

## Tensorflow Tutorial

```python
model = keras.Sequential([
  # 이미지에 있는 픽셀의 행을 펼쳐서 일렬로 늘린다. 학습되는 가중치는 없고 데이터를 변환하기만 한다.
  keras.layers.Flatten(input_shape=(28,28)), 
  # 밀집 연결 또는 완전 연결층이라고 부른다. 128개의 노드를 가진다.
  keras.layers.Dense(128,activation='relu'),
  # 10개의 노드의 소프트맥스 층이다. 이 층은 10개의 확률을 반환하고 변환된 값의 전체 합은 1이다. 각 노드는 현재 이미지가 10개 클래스 중 하나에 속할 확률을 출력한다.
  keras.layers.Dense(10, activation='softmax')
])

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy',metrics=['accuracy'])

#model.fit 메서드를 호출하면 모델이 훈련 데이터를 학습한다.
model.fit(train_images, train_labels, epochs=5)

#테스트 세트에서 모델의 성능을 비교한다.
test_loss, test_acc = model.evaluate(test_images, test_labels,verbose=2)

print('\n테스트 정확도:',test_acc)

predictions = model.predict(test_images)

```

## relu

기존에 사용하던 Sigmoid function 을  relu가 대체하게 된 이유 중 가장 큰 것이 Gradient Vanishing 문제이다. Sigmoid function 은 0에서 1사이의 값을 가지는데, gradient descent를 사용해 Backpropagation 수행시 layer를 지나면서 gradient(기울기) 를 계속 곱하므로, gradient 는 gradient는 0으로 수렴하게 된다. 따라서 layer가 많아지면 잘 작동하지 않게 된다.

따라서 이러한 문제를 해결하기위해 relu를 새로운 activation function으로 사용한다. relu는 입력값이 0보다 작으면 0이고 0보다 크면 입력값 그대로를 내보낸다. 



## softmax

3-class이상의 classification을 목적으로 하는 딥러닝 모델의 출력층에서 일반적으로 쓰이는 활성화 함수이다. 

합이 1이된다.



## epoch

인공 신경망에서 전체 데이터 셋에 대해 한 번 학습을 완료한 상태

## batch size

한 번의 batch 마다 주는 데이터 샘플의 size, 여기서 batch는 나눠진 데이터 셋을 뜻하며, iteration은 epoch를 나누어서 실행하는 횟수라고 생각하면 된다.
