<template lang="pug">
  #createModel
    ul
     //li(@click="clicked()") クリック
     li(@click="changeCamera('front')") 正面
     li(@click="changeCamera('side')") 横
     li(@click="changeCamera('back')") 背面
    // li {{aaa}}
</template>

<script lang="ts">
import { defineComponent, ref } from '@nuxtjs/composition-api'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

// オブジェクトの作成
const object = new THREE.Object3D()

const renderingModel = () => {
  //  シーンの作成
  const scene = new THREE.Scene()

  // レンダラーの作成
  const renderer = new THREE.WebGLRenderer({ alpha: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  document.body.appendChild(renderer.domElement)

  // カメラの作成
  const camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    1,
    500
  )
  camera.position.set(0, 0, 5)
  camera.lookAt(0, 0, 0)

  // ライトの作成
  const directionalLight = new THREE.DirectionalLight('#aaaaff', 1)
  directionalLight.position.set(0, 10, 10)

  // スカイボックスの作成
  const loader = new THREE.CubeTextureLoader()
  const texture = loader.load([
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/pos-x.jpg',
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/neg-x.jpg',
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/pos-y.jpg',
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/neg-y.jpg',
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/pos-z.jpg',
    'https://threejsfundamentals.org/threejs/resources/images/cubemaps/computer-history-museum/neg-z.jpg',
  ])

  // gltfモデルの読み込み
  const gltfLoader = new GLTFLoader()

  gltfLoader.load('/mutant.gltf', (modelData: any) => {
    const model = modelData.scene

    // Object3Dに入れる
    object.add(model)

    // 諸々の初期化
    object.scale.set(15, 15, 15) // スケール
    object.rotation.set(0, 0, 0) // 回転
    object.position.set(0, -1.5, 0) // 位置
  })

  // シーンにモデルを配置
  scene.add(object)

  // シーンにライトとスカイボックスを配置
  scene.add(directionalLight)
  scene.background = texture

  // ついでに環境光を配置して明るくする
  scene.add(new THREE.AmbientLight(0x222222))

  // シーンとカメラを元にレンダリングする
  renderer.render(scene, camera)

  // コントローラーの準備
  const controls: OrbitControls = new OrbitControls(camera, document.body)

  // アニメーションループの開始
  function animate() {
    requestAnimationFrame(animate)

    // object.rotation.y += 0.01

    controls.update()
    renderer.render(scene, camera)
  }

  animate()
}

export default defineComponent({
  setup() {
    renderingModel()

    const changeCamera = (direction: String) => {
      console.log(direction)
      switch (direction) {
        case 'front':
          object.rotation.set(0, 0, 0)
          break

        case 'side':
          object.rotation.set(0, 1.5, 0)
          break

        case 'back':
          object.rotation.set(0, 3, 0)
          break
      }
    }

    const aaa: any = ref(true)
    const clicked = () => {
      aaa.value === true ? (aaa.value = false) : (aaa.value = true)
      console.log(aaa.value)
    }

    return { aaa, clicked, changeCamera }
  },
})
</script>

<style lang="scss">
#createModel {
  background-color: #fff;
  margin-left: auto;

  ul {
    padding: 0 30px 0 0;
    text-align: left;

    li {
      list-style: none;
    }
  }
}
</style>
