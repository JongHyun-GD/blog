---
title: "[Raytracing in C]"
excerpt: "what is Phong Shading?"

categories:
  - CG 
tags:
  - Ray-tracing
last_modified_at: 2021-01-08
---
# Phong Shading

## Phong Shading?

Phong shading은 Bui Tuong Phong이 1970년대에 발표한 논문을 바탕으로 한 셰이딩 모델이다. Phong Shading은 크게 세가지 reflection을 구한 후 모두 더해서 결과값을 만들어낸다. 밑의 그림을 보면 쉽게 알 수 있다.

![Imgur](https://i.imgur.com/SaEZLRl.png)

## 3 Reflection, Ambient - Diffuse - Specular

Phong shading의 근간이 되는 세 개의 반사를 하나씩 알아보자.

1. Ambient
   Ambient는 쉽게 말해 항상 있는 반사다. 현실에서 정말 아무것도 보이지 않는 상황은 별로 없다. 이를 구현한 게 Ambient Reflection이다.
2. Diffuse
   Diffuse는 거친 표면의 반사라고 이해하면 쉽다. 빛을 강하게 받아 가장 밝을 때 본연의 색에 가장 가깝게 표현된다.
3. Specular
   이 반사는 매끈한 표면의 반사를 의미한다. 빛을 강하게 받는 부분은 하얀색으로 빛난다.

## Phong shading 구현

Ambient, Diffuse, Specular Reflection의 결과값을 각각 ka, kd, ks라고 한다면 한 점에 대한 Phong shading의 결과는 다음과 같다.

>  result = ka + kd + ks

하나씩 연산식을 알아보자.

### ka

ambient reflection은 정의한대로 어떤 상황에서도 나타나는 반사다. 그러므로 따로 어떠한 연산이 필요없다.

### kd

Diffuse reflection부터 벡터 연산이 필요하다. 하지만 phong shading의 강점 중 하나가 이 연산식 자체가 매우 간단하다는 점이다. 한 표면에 대해 만들어지는 벡터는 다음과 같다.

![Imgur](https://i.imgur.com/HH0bpPd.png)

N은 해당 point가 향하는 방향, normal vector를 의미한다. L은 해당 point에서 광원(lighting)으로의 방향이다. **어떤 표면이 빛을 가장 잘 받을 때는 언제일까? 그 표면이 광원을 향해 있을 때다.** 이 원리를 이용해보면 다음과 같은 사실을 알 수 있다.

> kd = N과 L의 유사도 * diffuse color

이 때, 벡터 간의 유사도는 dot product를 통해 알아낸다. N과 L이 완전히 같을 때, dot product는 1을 반환하고 완전히 반대를 향하면 -1을 반환한다. 이 때, 유사도가 0 이하인 것은 0과 같다. 완전히 반대라는 것은 표면의 반대편에 빛이 비추고 있다는 것인데, 이에 대한 연산은 많은 경우에 할 필요가 없기 때문이다. 이를 식에 적용하면 다음과 같다.

> kd = max(dot(N, L), 0) * diffuse color

### ks

ks를 연산하기 위해서는 두 개의 새로운 벡터가 필요하다. 일단 그림으로 살펴보자.

![Imgur](https://i.imgur.com/L6iGGcJ.png)

V는 해당 지점에서 카메라로의 벡터이고 H는 V와 L 사이각의 중심으로 뻗어나가는 벡터다. 이 계산식은 다음과 같다.

> H = V + L / || V + L ||

|| V + L ||은 V + L 벡터의 크기를 의미한다.

이 H가 핵심인데, **H와 N이 유사할수록 광원의 빛이 표면에 맞아 반사되어 우리 카메라에 들어올 것이다.** 이 특징을 이용한 게, Specular Reflection이다. 결국 kd의 계산식과 유사하게 ks를 표현할 수 있다.

> ks = max(dot(N, H), 0) * specular color

### Result

> R = ka + kd + ks
>
> ambient color + max(dot(N, L), 0) * diffuse color + max(dot(N, H), 0) * specular color

### 참고

모든 계산식에서 빛의 색깔은 감안하지 않았다. 빛의 색까지 계산하려면 kd와 ks를 계산할 때, 같이 곱해주면 된다.
