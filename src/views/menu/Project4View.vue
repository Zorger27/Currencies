<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURates from "@/components/other/NBURates.vue";

@Options({
  mixins: [openGraphMixin],
  data() {
    return {
      tableView: false
    }
  },
  mounted() {
    const mainTitle = 'Table / List';
    const title = 'Table / List';
    const metaDescription = 'Exchange rates from National Bank of Ukraine';
    const description = 'Exchange rates from National Bank of Ukraine';
    const imageUrl = 'https://currencies-zeta.vercel.app/assets/ogimage/bmp/project4.jpg';
    const url = 'https://currencies-zeta.vercel.app/project4';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
    this.setCanonical(url);
  },
  methods: {
    changeView() {
      this.tableView = !this.tableView;
    }
  },
  components: {NBURates, CurrentDate},
})
export default class Project4 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project4.name') }}</h1>
    <line></line>
    <CurrentDate></CurrentDate>
    <h1 class="bank">
      <a href="https://bank.gov.ua/ua/open-data/api-dev" title="In more detail..." target="_blank">
        {{ $t('nbu') }}
      </a> <i :title="[tableView ? 'Start viewing in List view' : 'Start viewing in Table view']" @click="changeView"><span :class="['fa', tableView ? 'fa-list' : 'fa-th']"></span></i>
    </h1>
    <NBURates :table-view="tableView"></NBURates>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(229, 255, 229), rgb(250, 247, 234)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}

  .bank {
    font-size: 2.5rem;
    a {text-decoration: none; color: rebeccapurple;}
    a:hover {color: cornflowerblue;}
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
