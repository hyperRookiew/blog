---
title: 【学习笔记】Three.js以及三维技术学习
categories:
  - [技术]
tags:
  - 前端
  - Three.js
date: 2022-02-13 15:23:52
---

> 背景知识
- 视频云项目又有了开发XR应用的需求了，就是实现各种AR、VR、全景视频直播等效果。Three.js是Web端进行3D展示的工具库，它对底层的WebGL进行了封装，大大简化了3D的开发。
- Three.js入门学习时候的难点在于对Three.js以及三维建模领域一些关键概念的理解，本文主要以梳理Three.js中的核心概念为主。对于Three.js中的各个核心概念，通过Best Practice的形式快速了解其用途和能力。
- 本文追求系统性，不追求细节。旨在快速建立对Three.js整体的概念了解，能够快速看懂和利用好官方文档。

### 一、基础知识
- 场景：scene，其中可以添加object
- 相机：camera，对scene中的内容进行拍摄
- 渲染器：render，对相机拍到的画面进行渲染
- 网格模型：mesh，
- 材质：material，
- 几何体：geometry，
- 点光源：PointLight，
- 环境光：AmbientLight，

<!--more-->

##### 场景Scene
- sence：场景

``` js
    var scene = new THREE.Scene(); // 创建场景
    scene.add(mesh); //网格模型添加到场景中
```

##### 几何体Geometry

- 几何框架

``` js
    //立方体 参数：长，宽，高
    var geometry = new THREE.BoxGeometry(100, 100, 100);
    // 球体 参数：半径60  经纬度细分数40,40
    var geometry = new THREE.SphereGeometry(60, 40, 40);
    //圆柱  参数：圆柱面顶部、底部直径50,50   高度100  圆周分段数
    var geometry = new THREE.CylinderGeometry(50, 50, 100, 25);
    // 正八面体
    var geometry = new THREE.OctahedronGeometry(50);
    // 正十二面体
    var geometry = new THREE.DodecahedronGeometry(50);
    // 正二十面体
    var geometry = new THREE.IcosahedronGeometry(50);
```

##### 材质Material
- 可以对几何体进行着色
- 本质：顶点着色器，可以被render解析加入计算

- 几种常用材质
  - 点材质
  - 线材质
  - 网格材质

```js
    //基础网格材质对象   不受光照影响  没有棱角感
    var material = new THREE.MeshBasicMaterial({
        color: 0x0000ff,
        wireframe:true,//线条模式渲染，线条编织成网
    });
    // 与光照计算  漫反射   产生棱角感
    var material = new THREE.MeshLambertMaterial({color: 0x0000ff }); 
    // 与光照计算  高光效果（镜面反射）产生棱角感
    var material = new THREE.MeshPhongMaterial({
        color: 0xff0000,
        specular:0x444444, // 高光部分的颜色
        shininess:30, // 高光部分的亮度
      side:THREE.DoubleSide, // 前面FrontSide  背面：BackSide 双面：DoubleSide
    });
    // 透明材质
    var material1 = new THREE.MeshLambertMaterial({
      color: 0x0000ff,//材质颜色
      transparent:true,//开启透明度
      opacity:0.5,//设置透明度具体值
    });
    // 点渲染模式
    var material = new THREE.PointsMaterial({
      color: 0xff0000,
      size: 5.0 //点对象像素尺寸
    });
    // 线条渲染模式
    var material=new THREE.LineBasicMaterial({
        color:0xff0000 //线条颜色
    });
```

##### 渲染模型
- 由几何体参数+材质参数组成
- 与材质类似，也有三种
  - 点模型：Point
  - 线模型：Line
  - 网格模型：Mesh

```js
    var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    scene.add(mesh); //网格模型添加到场景中

    mesh.rotateY(0.01);         //绕y轴旋转0.01弧度
    mesh.translateY(120);       //球体网格模型沿Y轴正方向平移120
    mesh.position.set(120,0,0); //设置mesh3模型对象的xyz坐标为120,0,0

    var point = new THREE.Points(geometry, material);
    var line = new THREE.Line(geometry, material); //线模型对象
```

- 复制模型

```js
    var geometry2 = new THREE.BoxGeometry(30, 30, 30); //创建一个立方体几何对象Geometry
    var material2 = new THREE.MeshLambertMaterial({
      color: 0xff00ff
    }); //材质对象Material
    var newMesh = new THREE.Mesh(geometry2, material2);
    // 复制mesh的位置、旋转、矩阵等属性(不包含geometry和material属性)
    newMesh.copy(mesh);
    //相比mesh而言，在平移
    newMesh.translateX(-50);
    scene.add(newMesh)
```

##### 相机Camera
- 远景相机（PerspectiveCamera），也就是类似于人眼观察的方式
  - 4个参数，决定了视椎体，用来裁剪视图，在该视锥体以外的物体将不会被渲染。
    - FOV：视角（field of view）
    - aspect_radio:相机拍摄面的长宽比（aspect ratio）,一般使用元素的宽除以高，否则会出现挤压变形。
    - near_plane: 近裁剪面
    - far_plane: 远裁剪面

```js
    var width = window.innerWidth; //窗口宽度
    var height = window.innerHeight; //窗口高度
    var aspect = width / height; //窗口宽高比
    var s = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
    // 创建相机对象
    var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
    // 创建远景相机,参数（FOV，aspect_radio,near_plane,far_plane）
    var camera = new THREE.PerspectiveCamera( 75, aspect , 0.1, 1000 );
    camera.position.set(200, 300, 200); //设置相机位置
    camera.lookAt(scene.position); //设置相机方向(指向的场景对象)
```

##### 渲染器Render
- WebGLRenderer为主，也有其他渲染器，但以兼容性为主
- WebGLRenderer，使用GPU进行渲染
- render负责解析数据，然后

```js
    var renderer = new THREE.WebGLRenderer();
    //设置渲染区域尺寸
    renderer.setSize(window.innerWidth, window.innerHeight);
    //设置背景颜色
    renderer.setClearColor(0xb9d3ff, 1); 
    // 把 renderer 元素添加到HTML文档中。这里是一个 <canvas> 元素，渲染器用来显示场景
    document.body.appendChild(renderer.domElement);
    //执行渲染操作
    renderer.render(scene, camera); // 只会渲染一次
    // 循环渲染
    function render() {
      cube.rotation.x += 0.1; // 实现动画
      cube.rotation.y += 0.1;
      requestAnimationFrame( render ); // window的函数，以每秒60次的频率来绘制场景
      renderer.render( scene, camera );
    }
    render();
```

- requestAnimationFrame函数：它用来替代 setInterval， 这个新接口具备多个优点，比如浏览器Tab切换后停止渲染以节约资源、和屏幕刷新同步避免无效刷新、在不支持该接口的浏览器中能安全回退为setInterval

### 二、相机控制与交互
- Three.js提供了OrbitControls.js，直接控制相机

```js
    //创建控件对象  相机对象camera作为参数   控件可以监听鼠标的变化，改变相机对象的属性
    var controls = new THREE.OrbitControls(camera,renderer.domElement);
    //监听鼠标事件，触发渲染函数，更新canvas画布渲染效果
    controls.addEventListener('change', render);
```

- 动画与交互同时控制

```js
    function render() {
      renderer.render(scene, camera); //执行渲染操作
      mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
      requestAnimationFrame(render);//请求再次执行渲染函数render，渲染下一帧
    }
    render();
    //创建控件对象  相机对象camera作为参数   控件可以监听鼠标的变化，改变相机对象的属性
    var controls = new THREE.OrbitControls(camera,renderer.domElement);
```

### 三、顶点
##### 顶点概念
- 顶点连接成线，线组成三角面，三角面组成更多类型的面，顶点具有position和color等基础属性
- BufferGeometry是所有几何体的基类，可以为其赋予多种属性，来实现不同的效果。
- 主要是通过顶点数据来确定渲染的。

```js
    var geometry = new THREE.BufferGeometry(); //创建一个Buffer类型几何体对象
    var vertices = new Float32Array([ //类型数组创建顶点数据
      0, 0, 0, //顶点1坐标
      50, 0, 0, //顶点2坐标
      0, 50, 0, //顶点3坐标
      0, 0, 10, //顶点4坐标
      0, 0, 60, //顶点5坐标
      50, 0, 10, //顶点6坐标
    ]);
    // 创建属性缓冲区对象
    var attribue = new THREE.BufferAttribute(vertices, 3); //3个为一组，表示一个顶点的xyz坐标
    // 设置几何体attributes属性的位置属性
    geometry.attributes.position = attribue;

    //类型数组创建顶点颜色color数据
    var colors = new Float32Array([
      1, 0, 0, //顶点1颜色
      0, 1, 0, //顶点2颜色
      0, 0, 1, //顶点3颜色
      1, 1, 0, //顶点4颜色
      0, 1, 1, //顶点5颜色
      1, 0, 1, //顶点6颜色
    ]);
    // 设置几何体attributes属性的颜色color属性
    geometry.attributes.color = new THREE.BufferAttribute(colors, 3); //3个为一组,表示一个顶点的颜色数据RGB
      //材质对象
    var material = new THREE.MeshBasicMaterial({
      // 使用顶点颜色数据渲染模型，不需要再定义color属性
      // color: 0xff0000,
      vertexColors: THREE.VertexColors, //以顶点颜色为准,即试用上边指定的颜色数组
    });
```

##### 顶点法向量
- 参与光照的计算，对物体最终所呈现的实际颜色的有影响的。
- 法向量对应的属性为normal

```js
  var normals = new Float32Array([
      0, 0, 1, //顶点1法向量
      0, 0, 1, //顶点2法向量
      0, 0, 1, //顶点3法向量

      0, 1, 0, //顶点4法向量
      0, 1, 0, //顶点5法向量
      0, 1, 0, //顶点6法向量
    ]);
    // 设置几何体attributes属性的位置normal属性
    geometry.attributes.normal = new THREE.BufferAttribute(normals, 3); //3个为一组,表示一个顶点的法向量数据
```

##### 顶点数据复用与索引
- 渲染一个四边形，实际需要6个顶点，是通过两个三角面实现的。
- 其中第二个三角面的两个顶点与第一个重合，所以可以复用。
- 通过顶点的索引属性来指定，实现顶点数据的复用，对应的属性名为index。

```js
      // Uint16Array类型数组创建顶点索引数据
    var indexes = new Uint16Array([
      0, 1, 2, 0, 2, 3,
    ])
    // 索引数据赋值给几何体的index属性
    geometry.index = new THREE.BufferAttribute(indexes, 1); //1个为一组
```

##### 设置Geometry的顶点
- 相比BufferGeomerty
  - 他们的属性组织架构是不同的，但实现的效果是相同的
  - BufferGeomerty是基于缩影的渲染，而Geometry是基于三角面的方式。
- 属性
  - vertices属性，顶点坐标数据
  - colors属性，顶点颜色数据

```js
    var geometry = new THREE.Geometry(); //声明一个几何体对象Geometry
    // Vector3向量对象表示顶点位置数据
    var p1 = new THREE.Vector3(50, 0, 0); //顶点1坐标
    var p2 = new THREE.Vector3(0, 70, 0); //顶点2坐标
    var p3 = new THREE.Vector3(80, 70, 0); //顶点3坐标
    //顶点坐标添加到geometry对象
    geometry.vertices.push(p1, p2, p3);
    // Color对象表示顶点颜色数据
    var color1 = new THREE.Color(0x00ff00); //顶点1颜色——绿色
    var color2 = new THREE.Color(0xff0000); //顶点2颜色——红色
    var color3 = new THREE.Color(0x0000ff); //顶点3颜色——蓝色
    //顶点颜色数据添加到geometry对象
    geometry.colors.push(color1, color2, color3);
```

##### Face3对象
- 定义几何体的三角面

```js
    var geometry = new THREE.Geometry(); //声明一个几何体对象Geometry
    var p1 = new THREE.Vector3(0, 0, 0); //顶点1坐标
    var p2 = new THREE.Vector3(0, 100, 0); //顶点2坐标
    var p3 = new THREE.Vector3(50, 0, 0); //顶点3坐标
    var p4 = new THREE.Vector3(0, 0, 100); //顶点4坐标
    //顶点坐标添加到geometry对象
    geometry.vertices.push(p1, p2, p3, p4);
    // Color对象表示顶点颜色数据
    var color1 = new THREE.Color(0x00ff00); //顶点1颜色——绿色
    var color2 = new THREE.Color(0xff0000); //顶点2颜色——红色
    var color3 = new THREE.Color(0x0000ff); //顶点3颜色——蓝色
    var color4 = new THREE.Color(0xffff00); //顶点3颜色——黄色
    //顶点颜色数据添加到geometry对象
    geometry.colors.push(color1, color2, color3, color4);
    // Face3构造函数创建一个三角面
    var face1 = new THREE.Face3(0, 1, 2);
    //设置三角面face1每个顶点的法向量
    var n1 = new THREE.Vector3(0, 0, -1);
    var n2 = new THREE.Vector3(0, 0, -1);
    var n3 = new THREE.Vector3(0, 0, -1);
    // 设置三角面Face3三个顶点的法向量
    face1.vertexNormals.push(n1, n2, n3);
    // 设置三角面face1三个顶点的颜色
    face1.vertexColors = [
      new THREE.Color(0xffff00),
      new THREE.Color(0xff00ff),
      new THREE.Color(0x00ffff),
    ]
    // 三角面2
    var face2 = new THREE.Face3(0, 2, 3);
    // 设置三角面法向量
    face2.normal = new THREE.Vector3(0, -1, 0);
    face2.color = new THREE.Color(0x00ff00);
    //三角面face1、face2添加到几何体中
    geometry.faces.push(face1, face2);
```

##### 几何体变换
- 其实就是对顶点坐标进行变换

```js
    // 几何体xyz三个方向都放大2倍
    geometry.scale(2, 2, 2);
    // 几何体沿着x轴平移50
    geometry.translate(50, 0, 0);
    // 几何体绕着x轴旋转45度
    geometry.rotateX(Math.PI / 4);
    // 居中：偏移的几何体居中
    geometry.center();
```

### 四、光源
- 可以与模型对象一起，添加到场景中
- 可以与材质的颜色进行计算，计算得到最终看到的颜色值
- 分类：
  - 环境光：无方向的光，其他都是有方向的
  - 点光源
  - 平行光源
  - 聚光光源

##### 点光源PointLight
- add插入场景中，render的时候会获取光源的信息进行光照计算


```js
    //点光源
    var point = new THREE.PointLight(0xffffff);
    point.position.set(400, 200, 300); //点光源位置
    scene.add(point); //点光源添加到场景中
```

##### 环境光AmbientLight
- 环境光颜色与网格模型的颜色进行RGB进行乘法运算
- 没有方向，所以没有明显的棱

```js
    //环境光
    var ambient = new THREE.AmbientLight(0x444444);
    scene.add(ambient);
```

##### 聚光

```js
    var spotLight = new THREE.SpotLight(0xffffff);
    // 设置聚光光源位置
    spotLight.position.set(200, 200, 200);
    // 聚光灯光源指向网格模型mesh2
    spotLight.target = mesh2;
    // 设置聚光光源发散角度
    spotLight.angle = Math.PI / 6
    scene.add(spotLight);//光对象添加到scene场景中
```

##### 平行光

```js
    var directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    // 设置光源的方向：通过光源position属性和目标指向对象的position属性计算
    // 注意：位置属性在这里不代表方向光的位置，你可以认为方向光没有位置
    directionalLight.position.set(80, 100, 50);
    // 方向光指向对象，可以不设置，默认的位置是0,0,0
    directionalLight.target = mesh2;
    scene.add(directionalLight);
```

##### 添加阴影
- 分两步
  - 1.开启模型的阴影
  - 2.添加一个接收阴影的模型

```js
    var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    scene.add(mesh); //网格模型添加到场景中
    mesh.castShadow = true;
    //创建一个平面几何体作为投影面
    var planeGeometry = new THREE.PlaneGeometry(300, 200);
    var planeMaterial = new THREE.MeshLambertMaterial({
      color: 0x999999
    }); //材质对象Material
    // 平面网格模型作为投影面
    var planeMesh = new THREE.Mesh(planeGeometry, planeMaterial); //网格模型对象Mesh
    scene.add(planeMesh); //网格模型添加到场景中
    planeMesh.receiveShadow = true;
    planeMesh.rotateX(-Math.PI / 2)
    planeMesh.position.y = -25;
```

### 五、组对象Group
- 目的是方便批量处理，可以将多个模型对象组合成一个Group，然后对这个Group进行统一的处理。

```js
    var group1 = new THREE.Group();
    var mesh = new THREE.Mesh(geometry, material);
    group1.add(mesh); //把网格模型插入到组group1中
    scene.add(group);
```

### 六、位置坐标
- 本地与世界作保
  - 本地坐标：该模型对象属性中设置的坐标
  - 世界坐标：一个模型对象可能还存在其他Group中，Group的坐标变化也会造成该模型对象的位置变化，最终该模型的坐标便是它的世界坐标。
- 获得世界坐标的方法

```js
  scene.updateMatrixWorld(true); // 更新世界坐标矩阵，不可缺少
  var worldPosition = new THREE.Vector3();
  mesh.getWorldPosition(worldPosition)
```

### 七、纹理贴图
##### 顶点UV映射数据
- 3D模型除了有顶点坐标、颜色等属性外，还有一个顶点纹理UV数据，UV负责实现外部纹理与模型顶点之间的映射
- 对于常用的立方体、球体、平面等模型，内部已经具有了UV数据。
- 对于更复杂的3D模型，通过外部modle.json文件导入的方式导入模型，该json中会包括UV数据。

##### 图片作为纹理
- 通过TextureLoader加载一个图片，然后把图片数据作为纹理数据，传递给map属性即可。

```js
    var geometry = new THREE.BoxGeometry(100, 100, 100); //立方体
    var geometry = new THREE.PlaneGeometry(204, 102); //矩形平面
    var geometry = new THREE.SphereGeometry(60, 25, 25); //球体
    // TextureLoader创建一个纹理加载器对象，可以加载图片作为几何体纹理
    var textureLoader = new THREE.TextureLoader();
    // 执行load方法，加载纹理贴图成功后，返回一个纹理对象Texture
    textureLoader.load('Earth.png', function (texture) {
      var material = new THREE.MeshLambertMaterial({
        map: texture,        // 设置纹理贴图：Texture对象作为材质map属性的属性值
      });
      var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
      scene.add(mesh); //网格模型添加到场景中
    })
```

##### 视频作为纹理

```js
    var geometry = new THREE.PlaneGeometry(108, 71); //矩形平面
    let video = document.createElement('video');     // 创建video对象
    video.src = "video.mp4"; // 设置视频地址
    video.autoplay = "autoplay"; //要设置播放
    // video对象作为VideoTexture参数创建纹理对象
    var texture = new THREE.VideoTexture(video)
    var material = new THREE.MeshPhongMaterial({
      map: texture, // 设置纹理贴图
    });
    var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    scene.add(mesh); //网格模型添加到场景中
```

##### 纹理整列
- 加载纹理之后，不断重复该纹理，即贴图效果。
- 实现草地效果

```js
    var geometry = new THREE.PlaneGeometry(1000, 1000); //矩形平面
    var texture = new THREE.TextureLoader().load("grass.jpg");
    texture.wrapS = THREE.RepeatWrapping;
    texture.wrapT = THREE.RepeatWrapping;
    texture.repeat.set(10, 10);     // uv两个方向纹理重复数量
    var material = new THREE.MeshLambertMaterial({
      map: texture,
    });
    var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    scene.add(mesh); //网格模型添加到场景中
    mesh.rotateX(-Math.PI / 2);

```

### 八、相机
- 相机拍摄的过程其实就是进行投影计算的过程，我们看到的物体实际是一个三维物体在一个方向上的投影
- 分类
  - 正投影：没有近大远小效果，适用于小场景，一般用于三维设计或者零件展示。可视区域是一个立方体。
  - 透视投影：近大远小效果，适用于大场景，比如游戏场景，可视区域是一个视锥体

##### 相机窗口自适应
- 透视相机自适应

```js
  window.onresize=function(){
    // 重置渲染器输出画布canvas尺寸
    renderer.setSize(window.innerWidth,window.innerHeight);
    // 全屏情况下：设置观察范围长宽比aspect为窗口宽高比
    camera.aspect = window.innerWidth/window.innerHeight;
    // 渲染器执行render方法的时候会读取相机对象的投影矩阵属性projectionMatrix
    // 但是不会每渲染一帧，就通过相机的属性计算投影矩阵(节约计算资源)
    // 如果相机的一些属性发生了变化，需要执行updateProjectionMatrix ()方法更新相机的投影矩阵
    camera.updateProjectionMatrix ();
  };
```
- 正交相机自适应

```js
    window.onresize=function(){
      // 重置渲染器输出画布canvas尺寸
      renderer.setSize(window.innerWidth,window.innerHeight);
      // 重置相机投影的相关参数
      k = window.innerWidth/window.innerHeight;//窗口宽高比
      camera.left = -s*k;
      camera.right = s*k;
      camera.top = s;
      camera.bottom = -s;
      // 渲染器执行render方法的时候会读取相机对象的投影矩阵属性projectionMatrix
      // 但是不会每渲染一帧，就通过相机的属性计算投影矩阵(节约计算资源)
      // 如果相机的一些属性发生了变化，需要执行updateProjectionMatrix ()方法更新相机的投影矩阵
      camera.updateProjectionMatrix ();
    };
```

### 九、精灵模型Sprite
##### 介绍
- 应用：
  - 大数据可视化
  - 下雨效果
- 渲染效果:无论相机如何变化，始终平行于桌面矩形区域，等价于一个正面永远朝向屏幕的PlaneGeometry。
- 相比与网格模型等，精灵模型只需要材质属性，而不需要几何体参数。

##### 精灵模型实现下雨

```js
    // 加载雨滴纹理贴图
    var textureTree = new THREE.TextureLoader().load("rain.png");
    // 创建一个组表示所有的雨滴
    var group = new THREE.Group();
    // 批量创建雨滴精灵模型
    for (let i = 0; i < 400; i++) {
      var spriteMaterial = new THREE.SpriteMaterial({
        map: textureTree, //设置精灵纹理贴图
      });
      // 创建精灵模型对象
      var sprite = new THREE.Sprite(spriteMaterial);
      group.add(sprite);
      // 控制精灵大小,
      sprite.scale.set(8, 10, 1); //// 只需要设置x、y两个分量就可以
      var k1 = Math.random() - 0.5;
      var k2 = Math.random() - 0.5;
      // 设置精灵模型位置，在一个长方体空间中随机分布
      sprite.position.set(200 * k1, 200 * Math.random(), 200 * k2)
    }
    scene.add(group);//雨滴群组插入场景中
```


### 相关链接
https://alligator.io/react/react-with-threejs
https://github.com/react-spring/react-three-fiber
https://github.com/zrysmt/react-threejs-app
https://github.com/chenjsh36/ThreeJSForFun
http://www.yanhuangxueyuan.com/Three.js/
https://wow.techbrood.com/fiddle/new