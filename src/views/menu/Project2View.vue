<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURatesCreep3d from "@/components/other/NBURatesCreep3d.vue";

@Options({
  mixins: [openGraphMixin],
  mounted() {
    const mainTitle = 'Courses v.3.0';
    const title = 'Courses v.3.0';
    const metaDescription = 'Exchange rates from National Bank of Ukraine';
    const description = 'Exchange rates from National Bank of Ukraine';
    const imageUrl = 'https://currencies-zeta.vercel.app/assets/ogimage/bmp/project2.jpg';
    const url = 'https://currencies-zeta.vercel.app/project2';

    this.setOpenGraphTags(metaDescription, title, description, imageUrl, url);
    this.setPageTitle(mainTitle);
  },
  // components: {CurrentDate},
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
      </a>
    </h1>
    <NBURatesCreep3d></NBURatesCreep3d>
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
  }
}
</style>
