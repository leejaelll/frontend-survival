# Textures

* 3D 모델의 표면에 적용되어 색상, 디테일 또는 기타 시각적 복잡성을 추가하는 이미지
* 더 현실적이고 시각적으로 흥미로운 3D 그래픽을 만드는 데 중요한 역할을 한다.&#x20;
* texture는 <mark style="color:red;">**PBR 원칙**</mark>을 따른다.&#x20;

{% hint style="info" %}
**PBR (Physically Based Rendering)**

: 3D 그래픽에서 빛이 물체의 표면과 상호작용하는 방식을 더 현실적으로 표현하는 방법



**Concepts**

1. PBR에서는 모든 표면이 빛을 반사한다고 가정한다. 반사되는 빛의 양과 방향은 표면이 거칠기나 질감에 따라 달라진다.&#x20;
2. PBR에는 금속성(Metallicity)과 거칠기(Roughness) 개념을 사용하여 재질의 특성을 정의한다.
3. 프레넬 효과(Fresnel effect) : 관찰자가 표면을 보는 각도에 따라 반사되는 빛의 양이 달라진다.&#x20;
{% endhint %}



texture를 로드하는 방법

```javascript
import imageSource from './color.jpg'


```
