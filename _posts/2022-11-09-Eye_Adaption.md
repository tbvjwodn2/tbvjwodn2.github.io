---
title: "Eye Adaption"
date: 2022-11-04 13:40:00
categories: lumen, ue5
---

Auto Exposure
화면 전체의 기본 Luminance를 18% Gray 즉 Final Color로는 50% Gray(127)로 맞추기 위해 있는것

예) 어두운 곳에 가면, 화면의 전체 평균 Luminance밝기를 50% Gray로 맞추는것
화면 전체의 Luminance값을 평균내서 나온 값이 40% Gray라고 가정하면, 50%가 되기까지 10%가 부족하기에, 전체 화면에 10%를 더해줘 50%의 평균 Luminance값을 갖도록 한다

예2) 밝은 곳에 가면, 화면의 전체 평균 Luminance밝기를 50% Gray로 맞추는것
 

한번 따라해보자
Post Process에서
- Min Ev100을 -10으로 놓고
- Max Ev100을 -20으로 설정한다
그리고 Directional Light Intensity를 조절하여, 흐린날씨를 만들어보자

아마 거의 불가능할것이다
Directional Light의 Intensity를 0.1이나 10이나, 50이나 차이가 없기 똑같이 그려질것이다

이것이 바로 Min Ev100과 Max Ev100의 기능이다
어떠한 밝기에 상관없이 화면의 평균 Luminance값을 50%로 맞추려고 하는것..
 
이렇게 되면, 생각날것이
UE4, UE5에서 갑자기 밝은 것을 보게되면, 주변 모든것들이 어두워지는 현상
또는 어두운 것을 보면 모든것들이 밝아지는 현상을 자주 목격한다

이런 것들은 너무 밝거나, 너무 어두워서 Exposure계산에 포함 시키고 싶지 않다
예를 들면, 태양의 밝기 값이 100인데 주변 평균 밝기 값이 10이라면, 평균치가 55가 되는데,
이렇게 되면, 평균 밝기가 높기때문에, Eye Adaption이 주변을 전부 어둡게 만들게된다

그렇다면 이제, 너무 밝은것, 또는 너무 어두운것들을 Exposure 계산에서 빼봐야하는데,
밑의 사진에서 Advanced의 Low Percent와 High Percent를 이용하는 방법이다

Low Percent와 High Percent는 Histogram Min Ev100과 Histogram Max Ev100의 값에 영향을 받는다

Histogram Min Ev100 ~~ Histogram Max Ev100를 조절하여 밑의 사진과 같이, Histogram자체를 Clipping해버려서, Exposure 계산 자체게 들어가지 

<img width="1008" alt="image" src="https://user-images.githubusercontent.com/45751396/200778406-6ef8132e-45aa-449e-8220-e6b69e1f509e.png">

<img width="500" alt="image" src="https://user-images.githubusercontent.com/45751396/200777860-07cda334-8d60-471c-ad10-a5347927ca0b.png">







Auto Exposure를 계산할때, 태양, 그림자가 Auto Exposure 계산에 들어가지 못하도록 하는 것이

Low Percent와 High Percent이다


![image](https://user-images.githubusercontent.com/45751396/200765729-abae3aea-f88a-46f5-91d3-c753e7e47016.png)
