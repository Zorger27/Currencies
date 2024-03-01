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
      contentWidth: 0,
      marqueeWidth: 0,
      animationOffset: 0,
    }
  },
  mounted() {
    this.fetchExchangeRates().then(() => {
      this.$nextTick(() => {
        const content = this.$refs.marquee.querySelector('.content');
        if (content) {
          // Дублирование содержимого для создания непрерывного эффекта
          content.innerHTML += content.innerHTML;
          // Пересчитываем ширину после дублирования
          this.calculateWidths();
        }
      });
    });
  },
  methods: {
    async fetchExchangeRates() {
      try {
        const response = await axios.get<ExchangeRate[]>(
          "https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json"
        );

        // Оставляем только выбранные валюты и удваиваем данные для создания непрерывного эффекта
        this.rates = response.data
          .filter(rate => ["USD", "EUR", "GBP", "AED", "TRY", "XAU", "XAG", "XPT", "XPD"].includes(rate.cc))
          .concat(response.data.filter(rate => ["USD", "EUR", "GBP", "AED", "TRY", "XAU", "XAG", "XPT", "XPD"].includes(rate.cc)));

        await this.$nextTick(this.calculateWidths);
        this.setupAnimationListener();
      } catch (error) {
        console.error(error);
      }
    },
    calculateWidths() {
      const marquee = this.$refs.marquee as HTMLElement | null;
      const content = marquee?.querySelector(".content") as HTMLElement | null;
      if (marquee && content) {
        this.marqueeWidth = marquee.offsetWidth;
        this.contentWidth = content.offsetWidth;
        this.animationOffset = this.marqueeWidth / 2;
      }
    },
    setupAnimationListener() {
      window.addEventListener("resize", this.calculateWidths);
      this.$nextTick(this.calculateWidths);
      this.animateMarquee();
    },
    animateMarquee() {
      const speed = 1; // Скорость анимации
      setInterval(() => {
        // Удвоение ширины контента, так как теперь у нас дублированный контент
        const fullContentWidth = this.contentWidth * 2;

        // Сброс анимации, когда достигнут конец дублированного контента
        if (this.animationOffset >= fullContentWidth) {
          this.animationOffset = 0;
        } else {
          this.animationOffset += speed;
        }

        const content = this.$refs.marquee?.querySelector(".content");
        if (content) {
          content.style.transform = `translateX(${-this.animationOffset}px)`;
        }
      }, 10);
    },
  },
  components: {},
})
export default class NBURatesCreepJS extends Vue {}
</script>

<template>
  <div class="container">
    <div ref="marquee" class="marquee">
      <div class="content">
        <div v-for="(rate, index) in rates" :key="`rate-${index}`" class="rates">
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
    overflow: hidden;
    position: relative;
    .content {
      white-space: nowrap;
      display: inline-block;
      .rates {
        display: inline-flex;
        font-size: 2rem;
        padding: 1rem;
        margin: 0.5rem 1rem;
        border: 1px solid lightgrey;
        border-radius: 5px;
        background-color: #f1f1f1;
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
}

@media(max-width: 1020px) {
  .marquee {
    .content {
      .rates {
        font-size: 1.6rem;
        padding: 0.8rem;
      }
    }
  }
}
@media (max-width: 768px) {
  .container {
    margin-bottom: 0.5rem;
    .marquee {
      .content {
        .rates {
          font-size: 1.5rem;
          padding: 0.6rem;
        }
      }
    }
  }
}
</style>