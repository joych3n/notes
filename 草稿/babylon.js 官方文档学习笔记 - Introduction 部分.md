# babylon.js 官方文档学习笔记 - Introduction 部分

## 第一

### 第一个场景

```javascript
const createScene = () => {
  const scene = new BABYLON.Scene(engine);
  const camera = new BABYLON.ArcRotateCamera(
    "camera",
    -Math.PI / 2,
    Math.PI / 2.5,
    3,
    new BABYLON.Vector3(0, 0, 0)
  );
  camera.attachControl(canvas, true);
  const light = new BABYLON.HemisphericLight(
    "light",
    new BABYLON.Vector3(0, 1, 0)
  );
  const box = BABYLON.MeshBuilder.CreateBox("box", {});
  return scene;
};
```

### 首次导入模型

```javascript
BABYLON.SceneLoader.ImportMeshAsync(model_name, folder_path, file_name, scene);
```

## 建设一个村庄

### 让世界接地

```javascript
const ground = BABYLON.MeshBuilder.CreateGround("ground", {
  width: 10,
  height: 10,
});
```

### 添加声音

```javascript
const sound = new BABYLON.Sound("name", "url to sound file", scene, null, {
  loop: true,
  autoplay: true,
});
```

### 放置并缩放网格

```javascript
const box = BABYLON.MeshBuilder.CreateBox("box", {
  width: 2,
  height: 1.5,
  depth: 3,
});
box.scaling.y = 1.5;
box.position.x = -2;
box.rotation.y = Math.PI / 4;
```

### 添加纹理

```javascript
const groundMat = new BABYLON.StandardMaterial("groundMat");
groundMat.diffuseColor = new BABYLON.Color3(0, 1, 0);
ground.material = groundMat; //Place the material property of the ground
```

### 房屋各面的材质

```javascript
// faceUV数组中，面的编号为0（背面）、1（正面）、2（右）、3（左）、4（顶部）和5（底部）。
const faceUV = [];
faceUV[0] = new BABYLON.Vector4(0.5, 0.0, 0.75, 1.0); //rear face
faceUV[1] = new BABYLON.Vector4(0.0, 0.0, 0.25, 1.0); //front face
faceUV[2] = new BABYLON.Vector4(0.25, 0, 0.5, 1.0); //right side
faceUV[3] = new BABYLON.Vector4(0.75, 0, 1.0, 1.0); //left side
const box = BABYLON.MeshBuilder.CreateBox("box", {
  faceUV: faceUV,
  wrap: true,
});
```

### 组合网格

```js
const combined = BABYLON.Mesh.MergeMeshes(Array_of_Meshes_to_Combine);
```

### 复制网格

```javascript
// 克隆
clonedHouse = house.clone("clonedHouse");
// 创建实例
instanceHouse = house.createInstance("instanceHouse");
```

## 乡村动画

### 网格父级

```js
meshChild.parent = meshParent;
```

### 动画基础

```js
// 1、创建动画对象，Animation方法的参数:名称、动画属性、每秒动画帧数、动画类型属性、循环模式
const animWheel = new BABYLON.Animation(
  "wheelAnimation",
  "rotation.y",
  30,
  BABYLON.Animation.ANIMATIONTYPE_FLOAT,
  BABYLON.Animation.ANIMATIONLOOPMODE_CYCLE
);
// 2、设置关键帧
const wheelKeys = [];
wheelKeys.push({
  frame: 0,
  value: 0,
});

wheelKeys.push({
  frame: 30,
  value: 2 * Math.PI,
});
// 3、将关键帧数组链接到动画
animWheel.setKeys(wheelKeys);
wheelRB.animations = [];
wheelRB.animations.push(animWheel);

scene.beginAnimation(wheelRB, 0, 30, true);
```

### 饶村散步

```javascript
// 将网格体沿其视点方向向前移动 6 个单位，参数依次为向右、向上和向前移动的距离
mesh.movePOV(0, 0, -6);
```

## 避免碰撞

### 避免车祸

```javascript
// 如果盒子边界网格 1 与盒子边界网格 2 重叠，则为真
mesh1.intersectsMesh(mesh2);
```

## 更好的环境

### 远山

```javascript
// 通过使用高度图来实现向地面网格添加垂直高度，白色区域是最高的部分，黑色区域是最低的部分。
const largeGround = BABYLON.MeshBuilder.CreateGroundFromHeightMap(
  "largeGround",
  "https://assets.babylonjs.com/environments/villageheightmap.png" /* url to height map */,
  { width: 150, height: 150, subdivisions: 20, minHeight: 0, maxHeight: 10 }
);
```

### 天空盒

```javascript
const skybox = BABYLON.MeshBuilder.CreateBox("skyBox", { size: 150 }, scene);
const skyboxMaterial = new BABYLON.StandardMaterial("skyBox", scene);
skyboxMaterial.backFaceCulling = false;
skyboxMaterial.reflectionTexture = new BABYLON.CubeTexture(
  "textures/skybox",
  scene
);
skyboxMaterial.reflectionTexture.coordinatesMode = BABYLON.Texture.SKYBOX_MODE;
skyboxMaterial.diffuseColor = new BABYLON.Color3(0, 0, 0);
skyboxMaterial.specularColor = new BABYLON.Color3(0, 0, 0);
skybox.material = skyboxMaterial;
```

### 精灵树

```javascript
// SpriteManager的参数是管理器的名称、图像的 url、精灵的最大数量、指定精灵的宽度和高度的对象
// 一、用二位图片创建一个始终面向相机的树
const spriteManagerTrees = new BABYLON.SpriteManager(
  "treesManager",
  "textures/palm.png" /* url to sprite */,
  2000,
  { width: 512, height: 1024 },
  scene
);
new BABYLON.Sprite("tree", spriteManagerTrees);

// 二、使用精灵图中的图像集合来生成动画。
const spriteManagerUFO = new BABYLON.SpriteManager(
  "UFOManager",
  "https://assets.babylonjs.com/environments/ufo.png" /* url to sprite */,
  1,
  { width: 128, height: 76 }
);
const ufo = new BABYLON.Sprite("ufo", spriteManagerUFO);
ufo.playAnimation(0, 16, true, 125);
```

## 构建粒子喷泉

### 车床改造的喷泉

```javascript
const fountainProfile = [
  new BABYLON.Vector3(0, 0, 0),
  new BABYLON.Vector3(10, 0, 0),
  new BABYLON.Vector3(10, 4, 0),
  new BABYLON.Vector3(8, 4, 0),
  new BABYLON.Vector3(8, 1, 0),
  new BABYLON.Vector3(1, 2, 0),
  new BABYLON.Vector3(1, 15, 0),
  new BABYLON.Vector3(3, 17, 0),
];
const fountain = BABYLON.MeshBuilder.CreateLathe(
  "fountain",
  { shape: fountainProfile, sideOrientation: BABYLON.Mesh.DOUBLESIDE },
  scene
);
```

### 粒子喷雾

```javascript
// 用于模拟火、烟、水甚至仙尘
const particleSystem = new BABYLON.ParticleSystem("particles", 5000, scene); //scene is optional
```

### 开启事件

```javascript
let switched = false; //on off flag
scene.onPointerObservable.add((pointerInfo) => {
  switch (pointerInfo.type) {
    case BABYLON.PointerEventTypes.POINTERDOWN:
      if (pointerInfo.pickInfo.hit) {
        pointerDown(pointerInfo.pickInfo.pickedMesh);
      }
      break;
  }
});
```

## 点亮黑夜

### 路灯

```javascript
const lampLight = new BABYLON.SpotLight(
  "name",
  position,
  direction,
  angle_of_spread,
  speed_of_disipation
);
lampLight.diffuse = BABYLON.Color3.Yellow();
```

### 添加阴影

```javascript
// 添加定向光
const shadowGenerator = new BABYLON.ShadowGenerator(1024, light);
// 添加一个可以投射阴影的网格
shadowGenerator.addShadowCaster(dude, true);
// 设置网格接受阴影
ground.receiveShadows = true;
```

## 观察世界的方式

### 环顾四周

```javascript
const camera = new BABYLON.ArcRotateCamera(
  "name",
  alpha_angle,
  beta_angle,
  radius,
  target_position
);
```

### 跟随那个角色

```javascript
const camera = new BABYLON.FollowCamera(
  "FollowCam",
  new BABYLON.Vector3(-6, 0, 0),
  scene
);
```

### 走向虚拟

```javascript
const xr = scene.createDefaultXRExperienceAsync();
```
