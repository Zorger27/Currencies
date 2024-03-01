<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {openGraphMixin} from "@/assets/ogimage/openGraphMixin";
import CurrentDate from "@/components/util/CurrentDate.vue";
import NBURates from "@/components/other/NBURates.vue";
import NBURatesCreepJS from "@/components/other/NBURatesCreepJS.vue";

@Options({
  mixins: [openGraphMixin],
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
  methods: {},
  components: {NBURatesCreepJS, NBURates, CurrentDate},
})
export default class Project1 extends Vue {
};
</script>

<template>
  <div class="container">
    <h1>{{ $t('project1.name') }}</h1>
    <line></line>
    <div><CurrentDate></CurrentDate></div>
    <NBURatesCreepJS class="creep"></NBURatesCreepJS>
    <div><NBURates></NBURates></div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  flex: 1 0 auto;
  background: linear-gradient(to bottom, rgb(255, 249, 229), rgb(255, 240, 244)) no-repeat center;
  h1 {font-size: 2.5rem;margin: 0.7rem auto;color: black;}
  .creep {background: none;}
}
@media(max-width: 1020px) {
  .container {
    h1 {font-size: 2.3rem;margin: 0.6rem auto;}
  }
}

@media (max-width: 768px) {
  .container {
    h1 {font-size: 2rem;margin: 0.5rem auto;}
  }
}
</style>
