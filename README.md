### Three.jsで太陽系を作ってみた。  

three.jsとは、、、  
WebGLはとても高度な技術である一方、APIは低レベルであるためそのまま使うにはどうしても冗長な準備を行う必要があります。  
一方で、JavaScriptライブラリーを通して高レベルなAPIとしてWebGLを利用する方法があります。  
JavaScriptライブラリーとしてはthree.js、Away3D.js、Babylon.jsなどが有名です。  
今回はthree.jsを使って太陽系をつくってみました。  
  

three.jsで宇宙を誕生させる！  
three.jsの流れとして、ワールドを作る必要があります。  
下記ようなの流れで作成していきます。  
  
* renderer作成  
* scene作成     
* camera作成    
* light作成     
* mesh（物体）作成
* 描画（アニメーション）

というような流れでワールドを作っていきます。  
screenにmesh,lightを追加していき、それをカメラで好きな角度から描画させるような感じです。  
ジオラマみたいなイメージでいいのかな。。。  
  

### ワールド作成  

render作成からlight作成までの基礎となるワールドは下記のコードできてしまいます。   
あとは、ライトやカメラなど種類がありますので後々設定していく感じです。  
  
```
// windowサイズ
　var width = window.innerWidth;
　var height = window.innerHeight;
// renderer作成  
  renderer = new THREE.WebGLRenderer();
  renderer.setSize(width, height);
  document.body.appendChild(renderer.domElement);
// scene作成     |
  scene = new THREE.Scene();
// camera作成
  camera = new THREE.PerspectiveCamera(75, width / height, 1, 5000);
  camera.position.z = 2000;
// ライト作成
  var DirectionaLight = new THREE.DirectionalLight(0xcccccc, 1.8);
  DirectionaLight.position.set(0, 0, 0);
  scene.add(DirectionaLight);
```

three.jsを初めて第一歩で奥行で苦戦したので、カメラについて説明します。  
PerspectiveCameraの引数としては、（画角,アスペクト比,手前側の画角?,奥行き)  
になります。第四引数の奥行をできるだけ大きめにとっておかないとオブジェクトを生成したときに  
エラーが出力されていないのにもかかわらず、真っ黒の画面ではまったのでここはわざと多めとって調節していくほうがいいかもしれません。  
  

### mesh（物体）作成

上記作成した空間に表示する物体を作成していきます。  
Geometry（形）とMaterial（材質）を作ってMeshに入れて3dの物体にしていきます。  
その物体をscreenに入れていきます。  
下記コードのようになります。  
  
```
// 形
  var geometryEarth = new THREE.SphereGeometry(60, 32, 32);
// 素材
  var materialEarth = new THREE.MeshBasicMaterial({
    map: THREE.ImageUtils.loadTexture('../images/earth.jpg')
  });
// 物体作成
  meshEarth = new THREE.Mesh(geometryEarth, materialEarth);
  scene.add(mesh)
```

ここは、つまずかずいけました。  

### 描画（アニメーション）

固定で表示する場合は、固定でポジションを指定することで表示することができますが、  
アニメーションにする場合は、少しずつずらして再帰的に描画することにより動かします。  

```

```

簡潔に説明するのが難しいため、簡単に説明してみましたが、three.jsで3dのページ作成はこんなような流れです。
コードは、説明するのが難しいので、githubのページを張っておきますので、大変世水辛いと思いますが、
コメントをヒントに読んでみてもらえるとうれしいです。

### ヘルパークラス
グリッドや、XYZ軸、ライトの位置を表示することができます。
しかも、下記の数行で表示できるんです。
カメラの画角やオブジェクトの位置や大きさを合わせるのに便利でした。
開発最中は、ずっと表示していました。

```
// グリッド表示
var gridHelper = new THREE.GridHelper(1500, 100);
scene.add(gridHelper);
// xyz軸表示
var axisHelper = new THREE.AxisHelper(1000);
scene.add(axisHelper);
// ライトヘルパー
 var DirectionaLightHelper = new THREE.DirectionalLightHelper(DirectionaLight, 20); 
 scene.add(DirectionaLightHelper);
```

### マウスぐりぐりホイール拡大
TrackballControlsモジュールは、驚きました。
これは、マウスでぐりぐり操作でき、ホイール拡大できるモジュールです。
threejsをダウンロードすると一緒に入っているもージュルnanoですが下記の二行を塚するだけで
マウスでグリグリ動かせるようになるんです。
もう楽しい！
```
//init部分に記述
  controls = new THREE.TrackballControls(camera);
  
// animation部分に記述
  controls.update();
```



## three js inspector(chromeプラグイン)
chromeのプラグインです。threejsで作成したオブジェクトを見ることができます。  
なんで表示されないのかわからないときに助かりました。



## 最後に
threejsを初めて触ってみまさいたが、はじめはとても敷居が高く感じており、
動く物体を作るだけで精一杯かなとおもっていたのですが、作り方の流れさえ理解してしまえば、
強力なAPIや面倒な処理を考える手間を省いてくれるモジュールが用意されており意外と簡単に3dページを作ることができました。
また、brenderでオブジェクト作ってしまえば描画して動かすだけですので、
もっと幅が広がって楽しめるのではないかと思いbrender勉強し始めようかなと思いました。
あと、物体を動かすのに三角関数を用いるのですが数学が苦手な私にはとても難問で土星作るより難しかったです。


## 完成品
#### ※けっこう重いです。(効果的に微妙だなと思いつつ影付けたら重くなりました.....)
#### [太陽系 https://mailei.github.io/](https://mailei.github.io/)

* リポジトリ
https://github.com/mailei/mailei.github.io
　　
　　
素材を拝借したサイト
http://planetpixelemporium.com/sun.html

参考サイト  
* http://4009.jp/post/2016-02-08-recovery_point/
* https://qiita.com/toshidev/items/08b6d15e23a59860b44a
* https://qiita.com/kingpanda/items/1c3a47765b40d6d62f46
* http://cx20.hatenablog.com/entry/2014/01/23/235440




















