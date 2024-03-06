<script>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
// import axios from "axios";

    // async fetchExchangeRates() {
    //   try {
    //     const response = await axios.get<ExchangeRate[]>("https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json");
    //     this.rates = response.data
    //       .filter(rate => ["USD", "EUR", "GBP", "AED", "TRY", "XAU", "XAG", "XPT", "XPD"].includes(rate.cc))
    //       .concat(response.data.filter(rate => ["USD", "EUR", "GBP", "AED", "TRY", "XAU", "XAG", "XPT", "XPD"].includes(rate.cc)));
    //     this.createExchangeRateObjects()
    //   } catch (error) {
    //     console.error(error);
    //   }
    // },
    // createExchangeRateObjects() {
    //   this.rates.forEach((rate: any, index: number) => {
    //     const geometry = new THREE.BoxGeometry(1, 1, 1);
    //     const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    //     const cube = new THREE.Mesh(geometry, material);
    //
    //     // Размещаем объекты с некоторым интервалом
    //     cube.position.x = index * 1.5 - this.rates.length / 2;
    //
    //     this.scene?.add(cube);
    //   });
    // },
export default {
  name: 'NBURatesCreep3d',
  setup() {
    const marquee = ref(null);
    let scene, camera, renderer, cube;

    const init = () => {
      // Создаем сцену
      scene = new THREE.Scene();

      // Создаем камеру
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 2.5;

      // Создаем рендерер
      renderer = new THREE.WebGLRenderer({ alpha: true });
      renderer.setSize(window.innerWidth, window.innerHeight);

      scene.add(camera);

      // Создаем геометрию и материал для куба
      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const materials = [
        new THREE.MeshBasicMaterial({ color: 0xff0000, transparent: true, opacity: 1 }),
        new THREE.MeshBasicMaterial({ color: 0x00ff00, transparent: true, opacity: 1 }),
        new THREE.MeshBasicMaterial({ color: 0x0000ff, transparent: true, opacity: 1 }),
        new THREE.MeshBasicMaterial({ color: 0xffff00, transparent: true, opacity: 1 }),
        new THREE.MeshBasicMaterial({ color: 0xff00ff, transparent: true, opacity: 1 }),
        new THREE.MeshBasicMaterial({ color: 0x00ffff, transparent: true, opacity: 1 }),
      ];
      cube = new THREE.Mesh(geometry, materials);

      // Добавляем куб на сцену
      scene.add(cube);

      // Добавляем рендерер в контейнер
      marquee.value.appendChild(renderer.domElement);

      // Обновляем сцену
      const animate = () => {
        requestAnimationFrame(animate);

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;

        renderer.render(scene, camera);
      };

      animate();
    };

    const onWindowResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      // renderer.render(scene, camera);
    };

    window.addEventListener('resize', onWindowResize);

    onMounted(() => {
      init();
      onWindowResize();
    });

    onUnmounted(() => {
      // Выполняем необходимые действия при удалении компонента
      // Например, очищаем ресурсы Three.js-server
      renderer.dispose();
    });

    return {
      marquee
    };
  },
}
</script>

<template>
  <div class="container">
    <div class="marquee" ref="marquee"></div>
<!--    <div class="scene-container" ref="canvasContainer"></div>-->
  </div>
</template>

<style lang="scss" scoped>
.container {
  margin-bottom: 1rem;
  overflow: hidden;
  position: relative;
  .marquee {
    max-height: 70vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}

@media(max-width: 1020px) {
  .marquee {}
}
@media (max-width: 768px) {
  .container {
    margin-bottom: 0.5rem;
  }
}
</style>
