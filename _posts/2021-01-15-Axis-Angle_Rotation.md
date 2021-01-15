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
# Axis-Angle-Rotation (Rodrigues' rotation formula)

## 상황과 목표

- **길이가 같은** 두 개의 벡터가 주어졌다.
  Ex) v1 = (0,0,1), v2 = (-0.8,0.4,0.3) -> 길이가 1로 동일한 두 벡터
- cameta orientation이 위와 같은 방식으로 rotation될 때, 카메라에서 쏘아지는 ray는 어떻게 회전시킬 수 있을까?
- 즉, 아래 그림에서 **src_rot를 구하는 게 목적**이다.
- ![Imgur](https://i.imgur.com/E0rqU6g.png)

## 임의의 축 회전

v1과 v2에 직각(perpendicular)인 벡터 a가 있다고 하자.
이 때, a는 v1과 v2를 외적(cross product)하면 구해진다. ([참고 동영상](https://www.youtube.com/watch?v=eu6i7WJeinw))
실제 계산식에서는 unit axis가 필요하므로 이것까지 적용한 식은 다음과 같다.
$$
\\frac{\\vec{v_1}\\times\\vec{v_2}}{|\\vec{v_1}||\\vec{v_2}|}
$$

---

축 A를 기준으로한 v1과 v2 사이의 각도는 다음의 식으로 구할 수 있다.
$$
\\cos{\\theta} =\\frac{\\vec{v_1}\\dot{}\\vec{v_2}}{ |\\vec{v_1}|*|\\vec{v_2}|}
$$

---

이제 돌리는 축도 알고, 돌려야 하는 각도도 알아냈다. 이제 실제로 돌리는 일만 남았다.

임의의 축을 기준으로 벡터를 돌리는 알고리즘은 **[Rodrigues' rotation formula](https://en.wikipedia.org/wiki/Rodrigues%27_rotation_formula)** 를 사용한다. 식은 다음과 같다.
$$
\\vec{v_{rot}} = (\\cos{\\theta})\\vec{v} + (\\sin{\\theta})(\\vec{a}\\times\\vec{v})+(1-\\cos{\\theta})(\\vec{a}\\dot{}\\vec{v})\\vec{a}
$$
앞에서 준비물은 전부 갖췄기 때문에 그냥 대입만 하면 알아서 회전된 벡터가 나온다.

## 코드 적용

함수 원형은 다음과 같다.

```c
t_vec3	axis_angle_rot(t_vec3 v1, t_vec3 v2, t_vec3 src)
```

v1은 basic orientation(여기선 (0,0,1))가 될 것이고 v2는 rt 파일에서 제시된 given orientation이다. **src**가 실제로 돌려야 할 vector다. 지금의 예시에선 카메라의 ray direction에 해당한다.

함수의 전체 코드는 다음과 같다.

~~~c
t_vec3	axis_angle_rot(t_vec3 v1, t_vec3 v2, t_vec3 src)
{
	t_vec3	axis;
	double	theta;

	axis = vec3_cross(v1, v2);
	theta = acos(vec3_dot(v1, v2) / (vec3_mag(v1) * vec3_mag(v2)));
	return (vec3_plus(vec3_plus(
		vec3_multi(cos(theta), src),
		vec3_multi(sin(theta), vec3_cross(axis, src))),
		vec3_multi((1 - cos(theta)) * vec3_dot(axis, src), axis)));
}
~~~

- axis를 계산할 때, 위에서 쓴 식과 달리 분모에 대한 식이 존재하지 않는다. 그 이유는 v1과 v2가 unit vector로 들어올 것이기 때문에 분모가 1 * 1이 되어서 계산할 필요가 없기 때문이다.
- C로 쓰다보니 함수가 많아져서 복잡해보이는데, 결국 식을 그대로 옮긴 것에 불과하다.

이 함수는 camera의 ray cast의 direction 계산과 hittable object의 회전에 쓰일 수 있다. Camera raycast에선 다음과 같이 쓰인다.

~~~c
// rd = ray direction
// param_1 = base direction
// param_2 = rt 파일에서 제시된 camera direction
// screen_pos = normalize된 screen position(위치 벡터이면서 방향 벡터이기도 하다.)
rd = axis_angle_rot(vec3_init(0, 0, 1), cam->dir, screen_pos);
~~~

이를 적용해서 카메라를 우측하단으로 비스듬하게 보게 하면 다음과 같은 결과물을 받을 수 있다.

![Imgur](https://i.imgur.com/0FUwhJc.png)
