# Textures

## `Texture`

* 3D 모델의 표면에 3D 모델의 표면에 적용되는 이미지
* 더 현실적이고 시각적으로 흥미로운 3D 그래픽을 만들 수 있다.&#x20;
* texture는 <mark style="color:red;">**PBR 원칙**</mark>을 따른다.&#x20;

{% hint style="info" %}
**PBR (Physically Based Rendering)**

: 3D 그래픽에서 빛이 물체의 표면과 상호작용하는 방식을 더 현실적으로 표현하는 방법



**Concepts**

1. PBR에서는 모든 표면이 빛을 반사한다고 가정한다. 반사되는 빛의 양과 방향은 표면이 거칠기나 질감에 따라 달라진다.&#x20;
2. PBR에는 금속성(Metallicity)과 거칠기(Roughness) 개념을 사용하여 재질의 특성을 정의한다.
3. 프레넬 효과(Fresnel effect) : 관찰자가 표면을 보는 각도에 따라 반사되는 빛의 양이 달라진다.&#x20;
{% endhint %}



## 텍스처 로드하기

`THREE.TextureLoader`를 사용하여 이미지를 텍스처로 로드할 수 있다.

```javascript
import imageSource from './color.jpg'

// 텍스처 로더 생성
const textureLoader = new THREE.TextureLoader();

// 텍스처 로드
const texture = textureLoader.load(imageSource);

// 기하학과 소재 생성
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ map: texture });

// 메쉬 생성
const cube = new THREE.Mesh(geometry, material);

// 씬에 추가
scene.add(cube);

// 애니메이션 루프 (렌더링)
function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}
animate();

```



## 텍스처 매핑 및 속성 조절하기

### 반복 및 랩핑

텍스트가 객체에 어떻게 매핑될지 조절할 수 있다. 👉🏻 `wrapS`와 `wrapT`

```javascript
texture.wrapS = THREE.RepeatWrapping;
texture.wrapT = THREE.RepeatWrapping;
texture.repeat.set(2, 2); // 텍스처를 두 번 반복
```

### 필터링

필터링 옵션으로 텍스처가 확대 또는 축소될 때 품질을 조절할 수 있다.&#x20;

#### `THREE.MipMapFilter`

* 텍스처가 다양한 해상도로 사전 계산된 여러 버전의 이미지를 사용하여 렌더링시 가장 적절한 해상도의 텍스처를 선택한다.&#x20;
* 텍스처가 멀리 있거나 작게 보일 때 유용하다.
* `THREE.NearestMipMapNearestFilter`, `THREE.NearestMipMapLinearFilter`, `THREE.LinearMipMapNearestFilter`, `THREE.LinearMipMapLinearFilter`

#### `THREE.NearestFilter`

* 텍스처의 픽셀과 가장 가까운 텍셀의 색상을 그대로 사용한다.&#x20;
* 텍스처가 "블록" 형태로 보이게 하는 특징이 있다. 👉🏻 주로 픽셀 아트나 레트로 스타일 그래픽에 적합

**`THREE.LinearFilter`**

* 선형 보간을 사용하여 텍스처를 필터링한다.&#x20;
* 텍스처가 더 부드럽고 자연스러워 보이는 효과가 있다.&#x20;

```javascript
texture.minFilter = THREE.LinearMipMapLinearFilter;
texture.magFilter = THREE.LinearFilter;
```

### 오프셋 중심

텍스처의 위치와 중심을 조절할 수 있다.&#x20;

```javascript
texture.offset.set(0.5, 0.5); // 텍스처를 오른쪽 위로 이동
texture.center.set(0.5, 0.5); // 중심을 텍스처의 중앙으로 설정
texture.rotation = Math.PI / 4; // 45도 회전
```



## 여러 텍스처 사용하기

하나의 객체에 여러 텍스처를 사용할 수도 있다.&#x20;

```javascript
const normalTexture = textureLoader.load('path/to/normalMap.jpg');
const material = new THREE.MeshStandardMaterial({
  map: texture,
  normalMap: normalTexture,
  metalness: 0.5,
  roughness: 0.5,
});
```



## 환경 맵

환경 맵은 객체가 주위 환경을 반사하거나 굴절하는 효과를 나타내는데 사용된다.&#x20;

```javascript
const envTexture = textureLoader.load('path/to/envMap.jpg');
const material = new THREE.MeshStandardMaterial({
  envMap: envTexture,
  metalness: 1,
  roughness: 0.1,
});
```



## 애니메이션 텍스처

텍스처 애니메이션을 만들기 위해 텍스처의 속성을 주기적으로 업데이트할 수 있다.&#x20;

```javascript
function animate() {
  requestAnimationFrame(animate);

  // 텍스처 애니메이션
  texture.offset.x += 0.01;
  
  renderer.render(scene, camera);
}
animate();
```





***

[https://threejs.org/docs/index.html#api/en/constants/Textures](https://threejs.org/docs/index.html#api/en/constants/Textures)&#x20;
