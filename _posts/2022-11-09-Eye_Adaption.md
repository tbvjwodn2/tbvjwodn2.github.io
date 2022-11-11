---
title: "Eye Adaption"
date: 2022-11-04 13:40:00
categories: lumen, ue5
---

## 화면의 밝기 (이 파트는 이해 안해도됨)

Exposure = 1/(2^(EV100 + Exposure Compensation))

EV100 = log2(Aperture^2 / Shutter Speed * 100/ISO)

>Exposure in the following formula defines the relationship between the scene surface luminance (L, which is measured in cd/m2) and pixel brightness (B) before the tonemapper and exposure compensation are applied.

B(픽셀 밝기) = Exposure * L



## Auto Exposure ( Eye Adaption )

화면 전체의 기본 Luminance를 18% Gray 즉 Final Color로는 화면 전체 평균 Luminance를 50% Gray(127)로 맞추기 위해 있는것입니다
~~18% Gray는 감마 보정이랑 관련있어서~~ 더 자세한 내용은 [여기](https://blog.naver.com/psy2993/90081371916)

예) 어두운 곳에 가면, 화면의 전체 평균 Luminance밝기를 50% Gray로 맞추는 형태라면
화면 전체의 Luminance값을 평균내서 나온 값이 40% Gray라고 가정하면, 50%가 되기까지 10%가 부족하기에, 전체 화면에 10%를 더해줘 50%의 평균 Luminance값을 갖도록 해줍니다

예2) 밝은 곳에 가면, 화면의 전체 평균 Luminance밝기를 50% Gray로 맞추는 형태라면
화면 전체의 평균 Luminance값이 70%라고했을때, 이를 50% Gray로 맞춰줍니다


위의 예시처럼 되려면 먼저 위에서 보았던 공식이 나오게됩니다
B(50%) = Exposure(x) * L(현재 화면 평균 밝기)

B의 50% Gray를 맞춰주기위해 L(현재 화면 평균 밝기)에 현재 스크린에서는 아직 모르는 미지수인 Exposure(x)를 곱해줘야 B에서 50%가 나오게됩니다



## 몇 가지 실험 해보기

#### 흐린날씨 만들기
흐린날씨 or 밝은 환경을 만들어보면 이해가 쉬워집니다

**Project Settings**

Extend Default luminance range in Auto Exposure Settings를 체크

**환경**

- Sky Atmosphere
- Directional Light
- Height Fog
- Sky Light

<img width="500" alt="image" src="https://user-images.githubusercontent.com/45751396/200777860-07cda334-8d60-471c-ad10-a5347927ca0b.png">
**Post Process**

Lens

- Min Ev100을 -10으로 설정
- Max Ev100을 -20으로 설정
그리고 Directional Light Intensity를 조절하여 흐린날씨를 만들어봅시다

아마 Directional Light를 엄청난 소수점까지 내려야 흐려질겁니다
Directional Light의 Intensity를 0.1이나 10이나, 50이나 차이가 없기 똑같이 보일겁니다

눈이 어두운곳, 밝은곳 가서 적응하는것처럼, Eye Adaption(Auto Exposure)기능이 그 기능을 하고 있기때문에
어떠한 밝기에 상관없이 화면의 평균 Luminance값을 50%로 맞추려고 합니다

이 기능이 Post Process의 Min Ev100과 Max Ev100의 역활 중 하나입니다

- Min Ev100으로 얼마나 어두운 범위까지 눈(화면)을 50% Gray로 맞출것인가
- Max Ev100으로 얼마나 밝은 범위까지 눈(화면)을 50% Gray로 맞출것인가


##### 하지만 쉽지않죠

UE4, UE5에서 갑자기 밝은 것을 보게되면, 주변 모든것들이 어두워지는 현상
또는 어두운 것을 보면 모든것들이 밝아지는 현상을 자주 목격합니다

이런 것들은 너무 밝거나, 너무 어두워서 Exposure계산에 포함 시키고 싶지 않게됩니다

예를 들면, 태양의 밝기 값이 100인데 주변 평균 밝기 값이 10이라면, 평균치가 55가 되는데,
이렇게 되면, 평균 밝기가 높기때문에, Eye Adaption이 주변을 전부 어둡게 만들게됩니다

- 너무 밝은것 or 너무 어두운것들을 Exposure 계산에서 제외 하려고하는데,
 
**Low Percen**와 **High Percent**를 이용하면 됩니다!

Histogram Min Ev100 ~~ Histogram Max Ev100를 조절하여 밑의 사진과 같이, Histogram자체를 Clipping해버려서, Exposure 계산 자체게 들어가지 않도록 할 수 있습니다

<img width="1008" alt="image" src="https://user-images.githubusercontent.com/45751396/200778406-6ef8132e-45aa-449e-8220-e6b69e1f509e.png">
