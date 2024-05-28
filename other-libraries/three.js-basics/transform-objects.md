# Transform objects

#### 4 properties to transform objects

* `poistion`
* `scale`
* `rotation`
* `quaternion`



**🔥 position**

* `x`: 왼쪽, 오른쪽으로 이동
* `y`: 위, 아래로 이동
* `z`: 앞, 뒤로 이동

```js
mesh.position.x = 0.7;
mesh.position.y = -0.6;
mesh.position.z = 1;

// set을 이용해 한번에 적용할 수 있다.
mesh.position.set(0.7, -0.6, 1);
```

axesHelper 클래스를 사용해 x,y,z 축을 쉽게 확인할 수 있다.

```js
const axesHelper = new THREE.AxesHelper(10);
scene.add(axesHelper);
```





**🔥 scale**

```js
mesh.scale.x = 2;
mesh.scale.y = 0.5;
mesh.scale.z = 0.5;
mesh.scale.set(2, 0.5, 0.5);
```





**🔥 rotate**

_👉🏻 `rotation` 혹은 `quaternion`, 둘 중에 하나를 변경하면 다른 방법도 자동으로 변경된다. 즉, rotation을 사용하면 해당 변경사항을 기반으로 quaternion을 자동으로 업데이트하고 반대로 quaternion을 변경하면 이를 기반으로 rotation을 자동으로 업데이트 한다._

{% hint style="info" %}
**Vector와 Euler**

* Vector
  * 크기와 방향을 가지는 수학적인 개념
  * 2D 공간에서는 (x, y) 좌표로 표현되며, 3D 공간에서는 (x, y, z) 좌표로 표현
  * 공간에서 한 지점에서 다른 지점으로의 방향과 거리를 표현
* Euler
  * 객체의 회전을 표현하는 방법 중 하나로, 세 개의 각도를 사용하여 객체를 회전
  * Roll, Pitch, Yaw라고도 불리는 세 축(x, y, z)을 기준으로 객체를 회전 👉🏻 각 축의 회전량을 나타내며, 객체가 각 축을 따라 얼마나 회전했는지 나타냄
{% endhint %}

_rotation_

```js
mesh.rotation.reorder('YXZ');
mesh.rotation.y = Math.PI; // 180도 회전
mesh.rotation.x = Math.PI * 0.25; // 45도 회전
```

👉🏻 _rotation은 gimbal lock이라는 문제를 유발할 수 있다. 이를 해결하기 위해 quaternion을 사용한다._



_quaternion_

```js
camera.lookAt(new THREE.Vector3(0, -1, 0));
```



***

```js
const group = new THREE.Group();
group.scale.y = 3;
group.rotation.y = 0.19;
scene.add(group);

const cube1 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube1.position.x = -1.5;
group.add(cube1);

const cube2 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube2.position.x = 0;
group.add(cube2);

const cube3 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube3.position.x = 1.5;
group.add(cube3);
```
