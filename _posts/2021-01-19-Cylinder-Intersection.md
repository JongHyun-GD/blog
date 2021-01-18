---
title: "[Raytracing in C]"
excerpt: "Axis Angle Rotation Explained"

categories:
  - CG
tags:
  - Ray-tracing 
last_modified_at: 2021-01-15T08:06:00-05:00
use_math: true
---
# Cynlinder Intersection

## 문제 상황

- ![Imgur](https://i.imgur.com/sBqQHKfm.png)
- 이 때, 어떻게 t 값을 구할 수 있을까?

## 식 전개

### 그림

![Imgur](https://i.imgur.com/uAAzDWrm.png)

### 준비

위 상황에서 모르는 값은 두 개 = h, t이다. 이를 위해 h와 t 사이의 관계식을 준비한다.

#### 관계식

![f1]\
![f2]

#### 풀어야 하는 식

![f3]

#### 전개

![f4]\
![f5]

- 이 때, 앞서 말한 바와 같이 P_x와 P_0은 다음과 같다.

![f6]\
![f7]

- 이를 적용하면

![f8]\
![f9]

- 식을 간단히 하기 위해 제곱식 안에 있는 t의 계수는 A, 나머지는 B로 치환하면

![f10]\
![f11]

- 이를 이용해 전개하면

![f13]

- 이 식에 근의 공식을 적용하면 그에 해당하는 a, b, c는 다음과 같다.

![f14]\
![f15]\
![f16]

- 이를 통해 해의 실수 여부를 판별한 후 구체적인 t값을 얻어낼 수 있다. h값도 t값을 대입만 하면 바로 나온다.
- h가 0이면 밑바닥의 노말을 주고 반대로 h가 높이와 같으면 윗바닥의 노말을 던져주면 된다. 그 사이는 P_x - P_0을 unit vector로 리턴한다.

[f1]: http://chart.apis.google.com/chart?cht=tx&chl=P_0=h\cdot{}N
[f2]: http://chart.apis.google.com/chart?cht=tx&chl=h=(P_x-C)\cdot{}N
[f3]: http://chart.apis.google.com/chart?cht=tx&chl=||P_x-P_0||^2=R^2
[f4]: http://chart.apis.google.com/chart?cht=tx&chl=(P_x-P_0)\cdot{}(P_x-P_0)=R^2
[f5]: http://chart.apis.google.com/chart?cht=tx&chl=(P_x-P_0)^2-R^2=0
[f6]: http://chart.apis.google.com/chart?cht=tx&chl=P_x=O%2BtD
[f7]: http://chart.apis.google.com/chart?cht=tx&chl=P_0=((P_x-C)\cdot{}N)\cdot{}N
[f8]: http://chart.apis.google.com/chart?cht=tx&chl=(O%2BtD-(((O%2BtD-C)\cdot{}N)\cdot{}N)^2-R^2=0
[f9]: http://chart.apis.google.com/chart?cht=tx&chl=((D-D\cdot{}N^2)t%2B(O-O\cdot{}N^2%2BC\cdot{}N^2))^2-R^2=0
[f10]: http://chart.apis.google.com/chart?cht=tx&chl=A=(D-D\cdot{}N^2)
[f11]: http://chart.apis.google.com/chart?cht=tx&chl=B=(O-O\cdot{}N^2%2BC\cdot{}N^2)
[f13]: http://chart.apis.google.com/chart?cht=tx&chl=A^2t^2%2B(2AB)t%2BB^2-R^2=0
[f14]: http://chart.apis.google.com/chart?cht=tx&chl=a=A^2
[f15]: http://chart.apis.google.com/chart?cht=tx&chl=b=2AB
[f16]: http://chart.apis.google.com/chart?cht=tx&chl=c=B^2-R^2
