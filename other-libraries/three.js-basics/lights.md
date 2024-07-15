# Lights

### DirectionalLight

* 물체에 가로막히면 그림자를 만들어내는 직사광선&#x20;

```javascript
const directionalLight = new THREE.DirectionalLight(0xffffff, 5);
directionalLight.castShadow = true;
directionalLight.position.set(3, 4, 5);
directionalLight.lookAt(0, 0, 0); // optional
scene.add(directionalLight);
```

{% hint style="info" %}
#### LightHelper

: 빛이 어디서부터 시작하는지 확인할 수 있도록 보조선을 그려주는 역할을 한다.&#x20;
{% endhint %}

```
const directionalLightHelper = new Three.Directional
```

### HemisphereLight

* 위쪽과 아래쪽으로 다른 색의 빛을 내뿜는 특이한 무드등, 그림자를 만들어내지는 못한다.&#x20;

```javascript
const hemisphereLight = new THREE.HemisphereLight(0xb4a912, 0x12f34f, 5);
hemisphereLight.position.set(0, 1, 0);
hemisphereLight.lookAt(0, 0, 0);
scene.add(hemisphereLight);
```

{% hint style="danger" %}
Geometry가 color 값을 가지고 있다면 빛의 색상이 보이지 않을 수 있다.&#x20;
{% endhint %}

<div align="left">

<figure><img src="../../.gitbook/assets/스크린샷 2024-07-15 오후 1.03.16.png" alt=""><figcaption></figcaption></figure>

</div>

### PointLight

* 현실의 무드등과 비슷함, 그림자를 만들어낼 수 있음

### RectAreaLight

* 불꺼진 방에서 핸드폰을 켰을 때 나타나는 빛

### SpotLight

* 무대에 서 있는 배우에게 집중되는 조명

