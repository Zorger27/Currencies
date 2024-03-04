<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURates from "@/components/other/NBURates.vue";
import NBURatesCreepJS from "@/components/other/NBURatesCreepJS.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      tableView: false,
      cripView: true,
      speed: 1,
    }
  },
  mounted() {
    const mainTitle = 'Courses Newest';
    const title = 'Courses Newest';
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
    }
  },
  components: {NBURatesCreepJS, NBURates, CurrentDate},
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
      <a href="https://bank.gov.ua" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i @click="changeView"><span :class="['fa', tableView ? 'fa-list' : 'fa-th']"></span></i>
      <i @click="changeCrip"><span :class="['fa', cripView ? 'fa-check-circle' : 'fa-sack-dollar']"></span></i> <input
      v-show="cripView" type="range" v-model.number="speed" min="0" max="4" step="0.2" />
    </h1>
    <NBURatesCreepJS class="creep" :crip-view="cripView" :speed="speed"></NBURatesCreepJS>
    <NBURates :table-view="tableView"></NBURates>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 249, 229), rgb(255, 240, 244)) no-repeat center;

  h1 {
    font-size: 2.5rem;
    margin: 0.7rem auto;
    color: black;
  }

  .bank {
    font-size: 2.5rem;

    a {
      text-decoration: none;
      color: rebeccapurple;
    }

    a:hover {
      color: cornflowerblue;
    }
  }

  .creep {
    background: none;
  }
}

@media(max-width: 1020px) {
  .container {
    h1 {
      font-size: 2.3rem;
      margin: 0.6rem auto;
    }

    .bank {
      font-size: 2rem;
    }
  }
}

@media (max-width: 768px) {
  .container {
    h1 {
      font-size: 2rem;
      margin: 0.5rem auto;
    }

    .bank {
      font-size: 1.6rem;
    }
  }
}
</style>
