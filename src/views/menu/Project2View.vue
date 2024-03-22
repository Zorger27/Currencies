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
      <a href="https://bank.gov.ua/ua/open-data/api-dev" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i @click="changeCrip3d"><span :class="['fa', cripView3d ? 'fa-yin-yang' : 'fa-sack-dollar']"></span></i>
    </h1>
    <div class="creep3d">
      <NBURatesCreep3d :crip-view3d="cripView3d"></NBURatesCreep3d>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 240, 244), rgb(229, 251, 255)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    margin-bottom: 1rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep3d {
    max-height: 30vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    background: none;
  }
}
@media(max-width: 1020px) {
  .container {
    h1, .bank {font-size: 2.3rem;margin: 0.6rem auto;}
  }
}

@media (max-width: 768px) {
  .container {
    h1, .bank {font-size: 2rem;margin: 0.5rem auto;}
    .creep3d {
      max-height: 20vh;
    }
  }
}
</style>
