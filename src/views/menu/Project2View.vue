<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURatesCreep3d from "@/components/other/NBURatesCreep3d.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      cripView3d: true,
      speed3d: 1,
    }
  },
  mounted() {
    const mainTitle = 'Courses 3D';
    const title = 'Courses 3D';
    const metaDescription = 'Exchange rates from National Bank of Ukraine';
    const description = 'Exchange rates from National Bank of Ukraine';
    const imageUrl = 'https://currencies-zeta.vercel.app/assets/ogimage/bmp/project2.jpg';
    const url = 'https://currencies-zeta.vercel.app/project2';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeCrip3d() {
      this.cripView3d = !this.cripView3d;
    },
  },
  components: {NBURatesCreep3d, CurrentDate},
})
export default class Project2 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project2.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank">
      <a href="https://bank.gov.ua" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i @click="changeCrip3d"><span :class="['fa', cripView3d ? 'fa-yin-yang' : 'fa-piggy-bank']"></span></i> <input
      v-show="cripView3d" type="range" v-model.number="speed3d" min="0" max="4" step="0.2" />
    </h1>
    <div class="creep3d">
      <NBURatesCreep3d :crip-view3d="cripView3d" :speed3d="speed3d"></NBURatesCreep3d>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(229, 251, 255), rgb(255, 240, 244)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep3d {
    max-height: 20vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
@media(max-width: 1020px) {
  .container {
    h1 {font-size: 2.3rem;margin: 0.6rem auto;}
    .bank {font-size: 2rem;}
  }
}

@media (max-width: 768px) {
  .container {
    h1 {font-size: 2rem;margin: 0.5rem auto;}
    .bank {font-size: 1.6rem;}
    .creep3d {
      max-height: 15vh;
    }
  }
}
</style>
