---
title: "[Raytracing in C] FOV"
excerpt: "what is fov?"

categories:
  - Blog
tags:
  - Blog
last_modified_at: 2021-01-08T08:06:00-05:00
---
# [Raytracing in C] FOV

## FOV란?

FOV는 Field of View의 준말이고 한국어로는 **'시야각'**이라고 곧잘 번역한다. 3D 게임을 많이 해본 사람이라면 **포브값**이라고 들어봤을 수 있는데, FOV가 그 포브값이다. 이 글에서는 가독성을 위해 FOV라고 말하겠다.

FOV는 '카메라가 볼 수 있는 각도'라고 생각하면 편하다. 결국 어떤 각도이기 때문에 degree 단위로 표현할 수 있다. degree는 중고등과정에서 배워서 친숙한 '도' 단위다. degree 단위로 표현된 90은 직각을 의미한다.

## 그림으로 FOV 이해하기

ray tracing을 위해 ray cast하는 모습을 옆에서 보면 다음과 같다.

![Imgur](https://i.imgur.com/Egi5782.jpg)

**focal length**라는 새로운 게 튀어나왔는데, 이는 그림을 그릴 캔버스와 카메라 사이 거리를 의미할 뿐이다. 즉 focal length가 1이었다가 10으로 변하면 카메라와 캔버스 사이 거리가 멀어졌다는 걸 뜻한다.

사실, 위 그림에서 주목해야할 건 focal length가 아니라 canvas의 맨 중앙에 해당하는 좌표가 (0, 0)이라는 점이다. 이를 스크린 좌표계라고 하겠다. 이 스크린 좌표계는 컴퓨터 모니터 중심에 십자를 그려 좌표계를 그렸다고 생각하면 된다.

만약 모니터의 가로 세로 픽셀이 800 * 600이라고 해보자. 스크린 좌표계에서 y 최대값은 1.0으로, x 최대값은 가로를 세로로 나눈 값으로 설정한다. 즉, 800 *. 600이면 세로 최대는 1.0이고 가로 최대는 1.3333...이다.



> [질문 1]  (답은 맨 아래)
>
> 이를 그대로 적용해보면 모니터의 좌측하단에 해당하는 좌표는 무엇일까?



다시 FOV로 돌아와보자. 앞에서 FOV는 '카메라가 볼 수 있는 각도'라고 설명했다. 하지만 FOV는 x축과 y축에 따라 그 값이 다르다. 갑작스러울 수 있는데, 그 이유는 생각보다 간단하다. 보통의 경우, 모니터의 가로와 세로 크기가 다르기 때문이다. 앞에서 예시를 든 800 * 600만 하더라도 가로가 더 길었다. 그래서 y축에 해당하는 FOV를 앞으로는 y-FOV라고 부르겠다. 

ray tracing은 canvas의 매 픽셀에 해당하는 곳마다 ray를 쏴서 그 결과값으로 pixel을 찍어내는 과정이다. 즉 y-FOV는 다음 그림의 각도를 의미한다.

![Imgur](https://i.imgur.com/kBeFnmZ.jpg)

그렇다면 위 그림에서 **focal length가 짧아지거나 길어지면 어떻게 될까?** 이 질문이 FOV를 이해하는 데 가장 중요한 질문이다. 다시 한번 생각해보자.



> [질문 2]
>
> focal length가 길어지거나 짧아질 때, y-FOV는 커지는가? 작아지는가?



위 질문의 상황을 그림으로 나타내면 아래와 같다.

![Imgur](https://i.imgur.com/43C6nRz.jpg)

즉, focal length가 길어지면 필연적으로 FOV는 좁아진다. 반대로, focal length가 짧아지면 FOV는 커진다. 이 관계는 반대로도 작동한다. FOV를 좁아지게 하려면 focal length는 길어지고 FOV를 커지게 하려면 focal length는 짧아진다.

즉, FOV값이 주어지면 우리는 focal length를 결정할 수 있다.

![Imgur](https://i.imgur.com/8nvuy70.png)

![Imgur](https://i.imgur.com/RLij9aN.png)

![Imgur](https://i.imgur.com/92XyM8E.png)

위 수식으로 입력받은 fov를 토대로 focal length를 조절하고 이를 통해 fov를 적용할 수 있다.





##### 질문의 답

(-1.333..., -1)
