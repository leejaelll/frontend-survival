---
description: Three.js  이해하기
---

# Three.js Basics



<div data-full-width="false">

<figure><img src="../../.gitbook/assets/240327-1.svg" alt=""><figcaption></figcaption></figure>

</div>

* `Renderer` : `Scene`과 `Camera`를 `Renderer`에게 전달하면 2D 이미지로 캔버스에 렌더링한다.&#x20;
* scenegraph는 [`Scene`](https://threejs.org/docs/#api/en/scenes/Scene) object, multiple [`Mesh`](https://threejs.org/docs/#api/en/objects/Mesh) objects, [`Light`](https://threejs.org/docs/#api/en/lights/Light) objects, [`Group`](https://threejs.org/docs/#api/en/objects/Group), [`Object3D`](https://threejs.org/docs/#api/en/core/Object3D), and [`Camera`](https://threejs.org/docs/#api/en/cameras/Camera) objects가 포함된 트리 형태의 구조이다.&#x20;
* `Mesh` : 객체는 특정 Geometry와 특정 Material을 사용하여 그림을 표현한다.
* `Geometry` : 구, 정육면체, 평면, 개, 고양이, 인간, 나무, 건물 등과 같은 기하학적 모양의 일부를 나타낸다.
* `Material` : 기하학을 그리는 데 사용되는 표면 속성을 나타낸다. 또한 Material은 하나 이상의 Texture 객체를 참조할 수 있다.
* `Texture` : 일반적인 이미지, 이미지 파일에서 로드되거나 캔버스에서 생성되거나 다른 장면에서 렌더링된 이미지가 될 수 있다.
* `Light` : 다양한 종류의 조명
