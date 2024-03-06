<script>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import {FontLoader} from "three/examples/jsm/loaders/FontLoader";
import {TextGeometry} from "three/examples/jsm/geometries/TextGeometry";
import axios from 'axios';

export default {
  name: 'NBURatesCreep3d',
  setup() {
    const marquee = ref(null);
    let scene, camera, renderer, currencies = [];

    const fetchExchangeRates = async () => {
      try {
        const response = await axios.get("https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json");
        const selectedRates = response.data
          .filter(rate => ["USD", "EUR", "GBP", "AED", "TRY", "XAU", "XAG", "XPT", "XPD"].includes(rate.cc));

        selectedRates.forEach((rate, index) => {
          createCurrencyObject(rate, index);
        });
      } catch (error) {
        console.error(error);
      }
    };

    const createCurrencyObject = (rate, index) => {
      const currencyText = `${rate.cc} ${rate.rate.toFixed(2)}`;
      const loader = new FontLoader();

      loader.load('https://threejs.org/examples/fonts/helvetiker_regular.typeface.json', (font) => {
        const geometry = new TextGeometry(currencyText, {
          font: font,
          size: 0.1,
          height: 0.02,
        });
        const material = new THREE.MeshBasicMaterial({color: 0x00ff00});
        const currencyObject = new THREE.Mesh(geometry, material);

        currencyObject.position.x = index * 1.5 - currencies.length / 2;

        currencies.push(currencyObject);
        scene.add(currencyObject);
      });
    };

    const init = () => {
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 2.5;
      renderer = new THREE.WebGLRenderer({alpha: true});
      renderer.setSize(window.innerWidth, window.innerHeight);
      scene.add(camera);

      marquee.value.appendChild(renderer.domElement);

      fetchExchangeRates();
      animate();
    };

    const animate = () => {
      requestAnimationFrame(animate);

      currencies.forEach((currency) => {
        currency.rotation.x += 0.01;
        currency.rotation.y += 0.01;
      });

      renderer.render(scene, camera);
    };

    const onWindowResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();

      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    };


    window.addEventListener('resize', onWindowResize);

    onMounted(() => {
      init();
      onWindowResize();
    });

    onUnmounted(() => {
      renderer.dispose();
    });

    return {
      marquee
    };
  },
};
</script>

<template>
  <div class="container">
    <div class="marquee" ref="marquee"></div>
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
</style>
