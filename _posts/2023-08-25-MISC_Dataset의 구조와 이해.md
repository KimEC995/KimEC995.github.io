---
title: Jupyter Notebook 파일 업로드 테스트
author: Kimec995
date: 2023-08-25 08:38:00 +09:00
categories: [Blogging, Demo]
tags: [ipynb]
pin: true
math: true
mermaid: true
---
# 목차

목차테스트 - 문서 내부 링크 만들기

1. [Dataset의 구조와 이해](#dataset의-구조와-이해)

2. [머신러닝에서 데이터 셋 활용하기](#머신러닝에서-데이터-셋-활용하기)

---

# Dataset의 구조와 이해


머신러닝을 배우기 위해 Scikit-Learn 라이브러리를 공부하고 있다.
해당 라이브러리에는 기본으로 제공하는 Dataset들이 있는데 처음 보는 구조이다.

어떤 구조이길래 머신러닝에 적합한지, 또 인자는 어떤 식으로 구성되어있는지 알기 위해 이 글을 작성한다.

이미지 테스트중중중

- 테스트1 : 깃 링크
![image.png](assets/img/favicons/Testimg.png)


- 테스트2 : url 링크
![image-2.png](https://github.com/KimEC995/KimEC995.github.io/blob/main/assets/img/favicons/Testimg.png)

## Dataset의 정의

[위키](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%A3%8C_%EC%A7%91%ED%95%A9)

위키에서 정의한 바로는

'데이터셋' = '자료 집합' 이란 자료가 모여있는 형태를 말한다.

일반적으로 자료 집합은 하나의 데이터 베이스 테이블의 내용이나 하나의 통계적 자료 행렬과 일치하며 각 row는 제시된 자료 집합의 주어진 멤버와 일치한다.\
이 자료 집합은 변수 개개의 값들을 나열하는데, 이를테면 자료 집합의 각 멤버에 대한 물처의 높이와 무게를 들 수 있다. (후략)

요약하면 특정 작업을 위해 수많은 데이터들을 관련성 있게 모아놓은 것을 **데이터 셋** 이라고 하며 여러 형식으로 된 자료가 있다.

## Dataset의 특징

- **데이터 포인트** : 데이터 셋은 하나 이상의 데이터 포인트로 구성된다. 이는 개별적인 정보 단위를 나타낸다.\
    예를 들어 테이블의 행 혹은 이미지의 픽셀 값 등이 될 수 있다.

- **Feature=특성** : 데이터 셋은 하나 이상의 특성으로 설명된다.\
예를 들어 고객 데이터 셋의 경우 이름, 나이, 성별 등이 특성이 될 수 있다.

- **Label = 레이블 / Traget = 타겟** : 머신러닝에서 사용하는 데이터 셋은 포인트에 대한 정답 또는 목표가 있다. 이런 값을 레이블 또는 타겟이라고 하며 주로 지도학습(Supervised Learning)에서 사용한다.\
예를 들어 스팸메일을 분류하는 모델일 경우 메일이 스팸인지 / 아닌지 표기하는 레이블을 지닌다.

- **구조** : 데이터 셋은 구조화 된 형태일 수도 있고, 비구조화 된 형태일 수도 있다. \
구조화 된 경우는[데이터베이스 / CSV / 테이블] 등이 있고 \
비구조화 된 경우는[텍스트 / 이미지] 등 형태가 있다.

# 머신러닝에서 데이터 셋 활용하기

Scikit-Learn 라이브러리에서 빌트인 데이터 셋은 `sklearn.utils.Bunch` 라는 구조로 활용한다.

딕셔너리와 유사한 구조로 key-value 형식을 취하기에 몇 가지 key는 알아두면 좋다.

### 공통 key

> 사용 : XXX.keyname

- data : 데이터 표기. Numpy Array 타입.

- traget : Label(목표) 데이터. Numpy Array 타입.

- feature_names : Feature(특성) 데이터 이름.

- target_names : Label(목표) 데이터의 이름.

- DESCT : 데이터셋 설명

- filename : 데이터셋 파일 저장 위치

### 관련 명령어
다양한 데이터에는 다양한 key들이 존재 할 것이다.
그걸 다 욀수는 없으니 관련 명령어를 활용하자.

- XXX.keys() : Key 리스트를 보여준다.

### 예제
글만 봐선 모르겠다.
아이리스 데이터로 확인하자


```python
from sklearn.datasets import load_iris

iris = load_iris()
```


```python
# 공통key
print("Data :", iris.data[:1],"\n")
print("Target :", iris.target[1],"\n")
print("Feature Names :", iris.feature_names,"\n")
print("Target Names :", iris.target_names,"\n")
print("데이터셋 설명 :", iris.DESCR,"\n")
print("파일저장위치 :", iris.filename,"\n")

```

    Data : [[5.1 3.5 1.4 0.2]] 
    
    Target : 0 
    
    Feature Names : ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)'] 
    
    Target Names : ['setosa' 'versicolor' 'virginica'] 
    
    데이터셋 설명 : .. _iris_dataset:
    
    Iris plants dataset
    --------------------
    
    **Data Set Characteristics:**
    
        :Number of Instances: 150 (50 in each of three classes)
        :Number of Attributes: 4 numeric, predictive attributes and the class
        :Attribute Information:
            - sepal length in cm
            - sepal width in cm
            - petal length in cm
            - petal width in cm
            - class:
                    - Iris-Setosa
                    - Iris-Versicolour
                    - Iris-Virginica
                    
        :Summary Statistics:
    
        ============== ==== ==== ======= ===== ====================
                        Min  Max   Mean    SD   Class Correlation
        ============== ==== ==== ======= ===== ====================
        sepal length:   4.3  7.9   5.84   0.83    0.7826
        sepal width:    2.0  4.4   3.05   0.43   -0.4194
        petal length:   1.0  6.9   3.76   1.76    0.9490  (high!)
        petal width:    0.1  2.5   1.20   0.76    0.9565  (high!)
        ============== ==== ==== ======= ===== ====================
    
        :Missing Attribute Values: None
        :Class Distribution: 33.3% for each of 3 classes.
        :Creator: R.A. Fisher
        :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)
        :Date: July, 1988
    
    The famous Iris database, first used by Sir R.A. Fisher. The dataset is taken
    from Fisher's paper. Note that it's the same as in R, but not as in the UCI
    Machine Learning Repository, which has two wrong data points.
    
    This is perhaps the best known database to be found in the
    pattern recognition literature.  Fisher's paper is a classic in the field and
    is referenced frequently to this day.  (See Duda & Hart, for example.)  The
    data set contains 3 classes of 50 instances each, where each class refers to a
    type of iris plant.  One class is linearly separable from the other 2; the
    latter are NOT linearly separable from each other.
    
    .. topic:: References
    
       - Fisher, R.A. "The use of multiple measurements in taxonomic problems"
         Annual Eugenics, 7, Part II, 179-188 (1936); also in "Contributions to
         Mathematical Statistics" (John Wiley, NY, 1950).
       - Duda, R.O., & Hart, P.E. (1973) Pattern Classification and Scene Analysis.
         (Q327.D83) John Wiley & Sons.  ISBN 0-471-22361-1.  See page 218.
       - Dasarathy, B.V. (1980) "Nosing Around the Neighborhood: A New System
         Structure and Classification Rule for Recognition in Partially Exposed
         Environments".  IEEE Transactions on Pattern Analysis and Machine
         Intelligence, Vol. PAMI-2, No. 1, 67-71.
       - Gates, G.W. (1972) "The Reduced Nearest Neighbor Rule".  IEEE Transactions
         on Information Theory, May 1972, 431-433.
       - See also: 1988 MLC Proceedings, 54-64.  Cheeseman et al"s AUTOCLASS II
         conceptual clustering system finds 3 classes in the data.
       - Many, many more ... 
    
    파일저장위치 : iris.csv 
    
    


```python
# 관련명령어
print("Key :", iris.keys(),"\n")
```

    Key : dict_keys(['data', 'target', 'frame', 'target_names', 'DESCR', 'feature_names', 'filename', 'data_module']) 
    
    

## DataFrame 으로 바꾸기

그 몇 주 잠깐 봤다고 이젠 DF가 더 익숙하다.
변환해보자


```python
import pandas as pd

df = pd.DataFrame(iris.data, columns=iris.feature_names)
df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



이제부터는 그냥 DataFrame이다.

이제 열심히 절심히 활용 해 보고 새로 알때마다 추가하자!
