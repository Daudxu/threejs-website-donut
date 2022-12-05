<template>
  <div class="content">
    <canvas class="webgl"></canvas>
    <div class="loading-bar"></div>

    <section class="one">
      <div class="container">
            <div class="hero">
                <h2>Abs are Cool.</h2>
                <h3>But have you ever tried donuts?</h3>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Doloremque aliquid culpa, <br>
                    providentvoluptatem excepturi id in quasi ea hic voluptate dicta amet explicabo <br>
                    expedita ratione velit modi. Nisiquaerat illum amet quisquam iusto perferendis <br>
                    ducimus aspernatur quia, repellendus beatae fugiat!</p>

            </div>
        </div>
    </section>
    <section class="two">
      <div class="container">
            <div class="hero">
                <h2>How we do</h2>
                <h3>Experiment width tasty <br>donuts recipe everytime</h3>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Doloremque aliquid culpa, <br>
                    providentvoluptatem excepturi id in quasi ea hic voluptate dicta amet explicabo <br>
                    expedita ratione velit modi. Nisiquaerat illum amet quisquam iusto perferendis <br>
                    ducimus aspernatur quia, repellendus beatae fugiat!</p>

            </div>
        </div>
    </section>

    <section class="three">
                <h1>HAPPY DONUT.</h1>
    </section>
  </div>
</template>

<script setup>
 import * as THREE from "three"
 import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
 import gsap from "gsap"
 import { onMounted, ref } from "vue";
//  import bg1 from "./assets/marcin-jozwiak-Jmsw8wYy7Ig-unsplash.jpg"
// import bg2 from "./assets/michael-olsen--djS1aPrSr4-unsplash.jpg"

 let   scene, camera, renderer, geometry, material, mesh, container, count, donut;

 let threeBgDom = ref(null)

 const clock = new THREE.Clock();
 const loadingBarElement = document.querySelector('.loading-bar')
 const bodyElement = document.querySelector('body')
 const loadingManager = new THREE.LoadingManager(
    () => {
        window.setTimeout(() => {
            gsap.to(overlayMaterial.uniforms.uAlpha, {
                duration: 3,
                value: 0,
                delay: 1
            })
            gsap.to(overlayMaterial.uniforms.uAlpha, {
                duration: 3,
                value: 0,
                delay: 1
            })

            loadingBarElement.classList.add('ended')
            bodyElement.classList.add('loaded')
            loadingBarElement.style.transform = ''

        }, 500)
    },
    (itemUrl, itemsLoaded, itemsTotal) => {
        console.log(itemUrl, itemsLoaded, itemsTotal)
        const progressRatio = itemsLoaded / itemsTotal
        loadingBarElement.style.transform = `scaleX(${progressRatio})`
        console.log(progressRatio)
    },
    () => {

    }
)
 const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
 }

 onMounted(()=>{
    // 实例化场景
    scene = new THREE.Scene()

    // 实例化相机
    camera = new THREE.PerspectiveCamera(70, window.innerWidth/window.innerHeight, 0.1, 1000)
    camera.position.z = 5;
    scene.add(camera)
    
    // 模型加载
    const gltfLoader = new GLTFLoader(loadingManager)
    gltfLoader.load(
    './assets/donut/scene.gltf',
    (gltf) => {
        console.log(gltf);

        donut = gltf.scene

        const radius = 8.5

        donut.position.x = 1.5;

        donut.rotation.x = Math.PI * 0.2
        donut.rotation.z = Math.PI * 0.15

        donut.scale.set(radius, radius, radius)

        scene.add(donut)
    },
    (progress) => {
        console.log(progress);
    },
    (error) => {
        console.error(error);
    }
)

    // 渲染器
    renderer = new THREE.WebGL1Renderer();
    renderer.shadowMap.enabled = true
    renderer.shadowMap.type = THREE.PCFSoftShadowMap
    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

    // container = threeBgDom.value.appendChild(renderer.domElement)
    // // 材质
    // const loader = new THREE.TextureLoader()
    // geometry = new THREE.PlaneGeometry(17, 8, 15, 9);
    // material = new THREE.MeshBasicMaterial({
    //    map: loader.load(bg2)
    // })
    // mesh = new THREE.Mesh(geometry, material)
    // scene.add(mesh)


    
    // count = geometry.attributes.position.count; 
    // animate()
    window.addEventListener('scroll', () => {

scrollY = window.scrollY
const newSection = Math.round(scrollY / sizes.height)

console.log(newSection);

if (newSection != currentSection) {
    currentSection = newSection

    if (!!donut) {
        gsap.to(
            donut.rotation, {
                duration: 1.5,
                ease: 'power2.inOut',
                z: transformDonut[currentSection].rotationZ
            }
        )
        gsap.to(
            donut.position, {
                duration: 1.5,
                ease: 'power2.inOut',
                x: transformDonut[currentSection].positionX
            }
        )

        gsap.to(
            sphereShadow.position, {
                duration: 1.5,
                ease: 'power2.inOut',
                x: transformDonut[currentSection].positionX - 0.2
            }
        )
    }
}
})
 })
//  function animate() {
//     const time = clock.getElapsedTime();
//     for(let i = 0; i < count; i++){
//       const x = geometry.attributes.position.getX(i)
//       const y = geometry.attributes.position.getY(i)

//       const animi1 = 0.25 * Math.sin(x + time * 0.7)
//       const animi2 = 0.35 * Math.sin(x * 1 + time * 0.7)
//       const animi3 = 0.1 * Math.sin(y * 15 + time * 0.7)

//       // geometry.attributes.position.setZ(i, animi1);
//       // geometry.attributes.position.setZ(i, animi1 + animi2);
//       geometry.attributes.position.setZ(i, animi1 + animi2 + animi3);
//       geometry.computeVertexNormals();
//       geometry.attributes.position.needsUpdate = true
//     }
//     requestAnimationFrame(animate)
//     renderer.render(scene, camera)
//   }
  


</script>

<style scoped>

</style>
