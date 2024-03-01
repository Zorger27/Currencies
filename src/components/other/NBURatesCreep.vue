<script lang="ts">
import {Options, Vue} from "vue-class-component";
import axios from "axios";

interface ExchangeRate {
  cc: string
  txt: string
  rate: number
  exchangedate: string
}
@Options({
  data() {
    return {
      rates: [] as ExchangeRate[],
    }
  },
  mounted() {
    this.setupAnimationListener();
    this.fetchExchangeRates();
  },
  methods: {
    async fetchExchangeRates() {
      try {
        const response = await axios.get<ExchangeRate[]>(
          'https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json'
        )
        this.rates = response.data.filter(
          (rate) =>
            rate.cc === 'USD' ||
            rate.cc === 'EUR' ||
            rate.cc === 'GBP' ||
            rate.cc === 'AED' ||
            rate.cc === 'TRY' ||
            rate.cc === 'XAU' ||
            rate.cc === 'XAG' ||
            rate.cc === 'XPT' ||
            rate.cc === 'XPD'
        )
      } catch (error) {
        console.error(error)
      }
    },
    setupAnimationListener() {
      const marquee = this.$refs.marquee as HTMLElement; // Приведение типа для TypeScript
      if (marquee) {
        marquee.addEventListener("animationiteration", this.handleAnimationIteration);
      }
    },
    handleAnimationIteration() {
      this.fetchExchangeRates();
    },
  },
  components: {},
})
export default class NBURates extends Vue {}
</script>

<template>
  <div class="container">
    <div ref="marquee" class="marquee">
      <div v-for="(rate, index) in rates" :key="index" class="rates">
        <div class="message">
          <span class="name">{{ rate.txt }}</span>=<span class="price">{{ rate.rate.toFixed(2) }}</span>{{ $t('uah') }}
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  margin-bottom: 1rem;
  overflow: hidden;
  position: relative;
  .marquee {
    white-space: nowrap; // Запрет переноса на новую строку
    //animation: marquee 20s linear infinite;
    animation: marquee 40s linear infinite;
    .rates {
      display: inline-flex;
      font-size: 2rem;
      padding: 1rem;
      margin: 0.5rem 1rem;
      border: 1px solid lightgrey;
      border-radius: 5px;
      background-color: #f1f1f1;
      //box-shadow: 0 4px 8px rgba(0, 0, 0, 0.7);
      .message {
        .name {
          margin-right: 5px;
          color: deepskyblue;
        }
        .price {
          margin-right: 5px;
          margin-left: 5px;
          color: deeppink;
        }
      }
    }
  }
  @keyframes marquee {
    0% { transform: translateX(100%); }
    100% { transform: translateX(-100%); }
  }
  //@keyframes marquee {
  //  from {transform: translateX(0%);}
  //  to {transform: translateX(-50%);}
  //}
}

@media(max-width: 1020px) {
  .marquee {
    .rates {
      font-size: 1.6rem;
      padding: 0.8rem;
    }
  }
}
@media (max-width: 768px) {
  .container {
    margin-bottom: 0.5rem;
    .marquee {
      animation: marquee 5s linear infinite;
      .rates {
      font-size: 1.5rem;
      padding: 0.6rem;
      }
    }
  }
}
</style>