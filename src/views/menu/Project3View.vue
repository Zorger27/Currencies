<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURatesCreepJS from "@/components/other/NBURatesCreepJS.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      cripView: true,
      speed: 1,
    }
  },
  mounted() {
    const mainTitle = 'Creeping line';
    const title = 'Creeping line';
    const metaDescription = 'Exchange rates from National Bank of Ukraine';
    const description = 'Exchange rates from National Bank of Ukraine';
    const imageUrl = 'https://currencies-zeta.vercel.app/assets/ogimage/bmp/project3.jpg';
    const url = 'https://currencies-zeta.vercel.app/project3';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeCrip() {
      this.cripView = !this.cripView;
    },
  },
  components: {NBURatesCreepJS, CurrentDate},
})
export default class Project3 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project3.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank">
      <a href="https://bank.gov.ua/ua/open-data/api-dev" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i :title="[cripView ? 'Close Creeping line' : 'Start Creeping line']" @click="changeCrip"><span :class="['fa', cripView ? 'fa-face-grin-stars' : 'fa-wallet']"></span></i> <input
      title="Changing speed of Creeping line" v-show="cripView" type="range" v-model.number="speed" min="0" max="6" step="0.2" />
    </h1>
    <NBURatesCreepJS class="creep" :crip-view="cripView" :speed="speed"></NBURatesCreepJS>
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
  .creep {background: none;}
}
@media(max-width: 1020px) {
  .container {
    h1, .bank {font-size: 2.3rem;margin: 0.6rem auto;}
  }
}

@media (max-width: 768px) {
  .container {
    h1, .bank {font-size: 2rem;margin: 0.5rem auto;}
  }
}
</style>
