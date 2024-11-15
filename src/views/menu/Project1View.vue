<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURates from "@/components/other/NBURates.vue";
import NBURatesCreepJS from "@/components/other/NBURatesCreepJS.vue";
import NBURatesCreep3d from "@/components/other/NBURatesCreep3d.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      tableView: false,
      cripView: true,
      cripView3d: true,
      speed: 1,
    }
  },
  mounted() {
    const mainTitle = 'Three-in-One';
    const title = 'Three-in-One';
    const metaDescription = 'Exchange rates from National Bank of Ukraine';
    const description = 'Exchange rates from National Bank of Ukraine';
    const imageUrl = 'https://currencies-zeta.vercel.app/assets/ogimage/bmp/project1.jpg';
    const url = 'https://currencies-zeta.vercel.app';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  methods: {
    changeView() {
      this.tableView = !this.tableView;
    },
    changeCrip() {
      this.cripView = !this.cripView;
    },
    changeCrip3d() {
      this.cripView3d = !this.cripView3d;
    },
  },
  components: {NBURatesCreep3d, NBURatesCreepJS, NBURates, CurrentDate},
})
export default class Project1 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project1.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank">
      <a href="https://bank.gov.ua/ua/open-data/api-dev" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i :title="[tableView ? 'Start viewing in List view' : 'Start viewing in Table view']" @click="changeView"><span :class="['fa', tableView ? 'fa-list' : 'fa-th']"></span></i> <i
      :title="[cripView3d ? 'Close 3D Creeping line' : 'Start 3D Creeping line']" @click="changeCrip3d"><span :class="['fa', cripView3d ? 'fa-yin-yang' : 'fa-sack-dollar']"></span></i> <i
      :title="[cripView ? 'Close Creeping line' : 'Start Creeping line']" @click="changeCrip"> <span :class="['fa', cripView ? 'fa-face-grin-stars' : 'fa-wallet']"></span></i> <input
      title="Changing speed of Creeping line" v-show="cripView" type="range" v-model.number="speed" min="0" max="6" step="0.2" />
    </h1>
    <div class="creep3d">
      <NBURatesCreep3d :crip-view3d="cripView3d"></NBURatesCreep3d>
    </div>
    <NBURatesCreepJS class="creep" :crip-view="cripView" :speed="speed"></NBURatesCreepJS>
    <NBURates :table-view="tableView"></NBURates>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 249, 229), rgb(255, 240, 244)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    background: transparent;
    a {text-decoration: none;color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
  }
  .creep3d {
    background: transparent;
    max-height: 30vh;
    max-width: 100%;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .creep {
    background: transparent;
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
  }
}
</style>
