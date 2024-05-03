# 一、three

##### 1.1 基本组成

```
1. canvas
2. 场景
3. 相机
4. 物体
canvas添加：场景、相机
场景添加：物体

代码组成：
<script setup>
import * as THREE from 'three';

//场景
const scene = new THREE.Scene();
//相机
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
//渲染canvas
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

//物体
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
//材质
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
//创建网格
const cube = new THREE.Mesh( geometry, material );

//把物体添加到场景中
scene.add( cube );

//调整相机的位置：x,y,z
camera.position.z = 2;
camera.position.x = 0;
camera.position.y = 0;

function animate() {
	requestAnimationFrame( animate );

	cube.rotation.x += 0.01;
	cube.rotation.y += 0.01;

  //把场景和相机，添加到canvas中
	renderer.render( scene, camera );
}

animate();
</script>
```

##### 1.2 物体有几种情况render

```
1. 原本自带物体
2. 原本自带物体 + 贴图
	//几何体
  const geometry = new THREE.BoxGeometry( 1, 1, 1 );
  //贴图
  const texture = new THREE.TextureLoader().load( '/imgs/restroom/7_b.jpg' );
  //材质
  const material = new THREE.MeshBasicMaterial( { map: texture } );
  //创建网格
  const cube = new THREE.Mesh( geometry, material ); 
  //场景添加网格
  scene.add( cube );
3. 渲染c4d等模型
	<script setup>
    import { onMounted  } from 'vue';

    import * as THREE from 'three';
    import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
    const width = window.innerWidth;
    const heihgt = window.innerHeight;

    //场景
    const scene = new THREE.Scene();
    //canvas
    const renderer = new THREE.WebGLRenderer();
    //loader
    const loader = new GLTFLoader();
    //相机
    const camera = new THREE.PerspectiveCamera( 45, width/heihgt, 1, 1000 );

    //加载模型
    function gltfModel(){
      loader.load("./kizuna_ai/scene.gltf",
        function ( gltf ) {
          //场景中添加模型
          scene.add( gltf.scene );
        },
      )
    }

    //初始化方法
    function init(){
      //灯光
      const light = new THREE.AmbientLight( 0x404040 ); // 柔和的白光
      scene.add( light );

      //相机的位置
      camera.position.set(0,0,100);
    }

    //渲染元素
    function renderModel(){
      //渲染区存储
      renderer.setSize(width,heihgt);
      //执行渲染操作：canvas放入相机和场景
      renderer.render( scene , camera );
    }

    function render() {
      renderer.render(scene, camera);
      requestAnimationFrame(render);
    }


    onMounted(()=>{
      document.getElementById('my-three').appendChild( renderer.domElement );
      init();
      renderModel();
      gltfModel();
      render();
    })
    </script>
```



# 二、语音SDK

```
https://console.xfyun.cn/services/iat
```

