# Geometries

#### Geometry + Material = Mesh

basic material 만들기

```javascript
const material = new THREE.MeshStandardMaterial({ color: 0x44aa88 }); // MeshStandardMaterial: 빛을 받는 물체로 만들어줌
```

<div align="left">

<figure><img src="../../../.gitbook/assets/240328-1.png" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="danger" %}
material이 보이지 않는 이유 👉🏻 MeshstandardMaterial은 빛을 받는 물체이기 때문에 보이지 않는 것.
{% endhint %}



**Light 추가하기**

```javascript
// 직사광선 만들기
const directionalLight = new THREE.DirectionalLight(0xffffff, 5);
directionalLight.castShadow = true;
directionalLight.position.set(3, 4, 5);
directionalLight.lookAt(0, 0, 0);
scene.add(directionalLight);
```



### PlaneGeometry

```javascript
const floorGeometry = new THREE.PlaneGeometry(20, 20);
const floorMaterial = new THREE.MeshStandardMaterial({ color: 0xbbbbbb });
const floor = new THREE.Mesh(floorGeometry, floorMaterial);
floor.rotation.x = -Math.PI / 2; // 90도 회전
floor.position.y = 0;
floor.receiveShadow = true;
floor.castShadow = true;
floor.name = "floor";
scene.add(floor);
```



### CapsuleGeometry

```javascript
const capsuleGeometry = new THREE.CapsuleGeometry(1, 2, 20, 30);
const capsuleMaterial = new THREE.MeshStandardMaterial({ color: 0xffff00 });
const capsuleMesh = new THREE.Mesh(capsuleGeometry, capsuleMaterial);
capsuleMesh.position.set(3, 1.75, 0);
capsuleMesh.castShadow = true;
capsuleMesh.receiveShadow = true;
scene.add(capsuleMesh);
```



{% hint style="danger" %}
mesh에 그림자가 보이지 않는다면?

👉🏻 `renderer.shadowMap.enabled = true`를 적용해야한다.&#x20;
{% endhint %}



### &#x20;CylinderGeometry

```javascript
const cylinderGeometry = new THREE.CylinderGeometry(1, 1, 2);
const cylinderMaterial = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
const cylinderMesh = new THREE.Mesh(cylinderGeometry, cylinderMaterial);
cylinderMesh.position.set(-3, 1, 0);
cylinderMesh.castShadow = true;
cylinderMesh.receiveShadow = true;
scene.add(cylinderMesh);
```



### TorusGeometry

```javascript
const torusGeometry = new THREE.TorusGeometry(0.5, 0.1, 16, 100);
const torusMaterial = new THREE.MeshStandardMaterial({ color: 0x0000ff });
const torusMesh = new THREE.Mesh(torusGeometry, torusMaterial);
torusMesh.position.set(0, 0.5, 1);
torusMesh.castShadow = true;
torusMesh.receiveShadow = true;
scene.add(torusMesh);
```



### ShapeGeometry

```javascript
const starShape = new THREE.Shape();
starShape.moveTo(0, 1);
starShape.lineTo(0.2, 0.2);
starShape.lineTo(1, 0.2);
starShape.lineTo(0.4, -0.1);
starShape.lineTo(0.6, -1);
starShape.lineTo(0, -0.5);
starShape.lineTo(-0.6, -1);
starShape.lineTo(-0.4, -0.1);
starShape.lineTo(-1, 0.2);
starShape.lineTo(-0.2, 0.2);

const shapeGeometry = new THREE.ShapeGeometry(starShape);
const shapeMaterial = new THREE.MeshStandardMaterial({ color: 0xff00ff });
const shapeMesh = new THREE.Mesh(shapeGeometry, shapeMaterial);
shapeMesh.position.set(0, 1, 2);
scene.add(shapeMesh);
```



### ExtrudeGeometry

```javascript
// 3D 오브젝트로 만들기 위한 세팅
const extrudeSettings = {
  steps: 1, // 모양 확장에 있어서 값이 클수록 부드러운 형태가됨
  depth: 0.1, // shape를 입체로 만들때의 두께
  bevelEnabled: true, // 모서리를 둥글게 만들지 여부
  bevelThickness: 0.1, // bevelEnabled: true일 경우, 모서리 바벨링의 두께
  bevelSize: 0.3, // bevelEnabled: true일 경우, 모서리의 크기
  bevelSegments: 100, // bevelEnabled: true일 경우, 바벨링된 모서리를 얼마나 매끄럽게 나눌지 설정하는 값
};

const extrudeGeometry = new THREE.ExtrudeGeometry(starShape, extrudeSettings);
const extrudeMaterial = new THREE.MeshStandardMaterial({ color: 0x0ddaaf });
const extrudeMesh = new THREE.Mesh(extrudeGeometry, extrudeMaterial);
extrudeMesh.position.set(2, 1.3, 2);
extrudeMesh.castShadow = true;
extrudeMesh.receiveShadow = true;
scene.add(extrudeMesh);
```



### BufferGeometry

점의 형태로 표현 가능

```javascript
const numPoints = 1000; // 포인트의 수
const positions = new Float32Array(numPoints * 3); // 각 포인트의 3D 위치 (x, y, z)

for (let i = 0; i < numPoints; i++) {
  const x = (Math.random() - 0.5) * 1; // X 좌표를 무작위로 생성
  const y = (Math.random() - 0.5) * 1; // Y 좌표를 무작위로 생성
  const z = (Math.random() - 0.5) * 1; // Z 좌표를 무작위로 생성

  positions[i * 3] = x;
  positions[i * 3 + 1] = y;
  positions[i * 3 + 2] = z;
}

const bufferGeometry = new THREE.BufferGeometry();
bufferGeometry.setAttribute(
  "position",
  new THREE.BufferAttribute(positions, 3)
);

const pointsMaterial = new THREE.PointsMaterial({
  color: 0xffff00,
  size: 0.05,
});

const point = new THREE.Points(bufferGeometry, pointsMaterial);
point.position.set(0, 0, -5);
scene.add(point);
```

