<template>
  <div class="f-card">
    <div class="f-card__inner">
      <figure class="f-card__img-wrap">
        <img :src="iconSrc" alt="" />
      </figure>
      <div class="f-card__info-container">
        <h2 class="f-card__location-name">{{ f_card.location }}</h2>
        <div class="f-card__temperature-grid">
          <div class="f-card__temperature">{{ f_card.temperature }}</div>
          <div class="f-card__temperature-degree"></div>
          <div
            class="f-card__temperature-type --сelsius"
            :class="{ active: ttype === 'C' }"
            @click.prevent="switchTType($event, 'C')"
          >
            C
          </div>
          <div
            class="f-card__temperature-type --fahrenheit"
            :class="{ active: ttype === 'F' }"
            @click.prevent="switchTType($event, 'F')"
          >
            F
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FCard',
  props: {
    f_card: Object,
  },
  data() {
    return {
      ttype: 'C',
      icon: '021-sun',
    }
  },
  computed: {
    iconSrc: function () {
      return require(`${process.env.W_ICONS}${this.icon}.svg`)
    },
  },
  methods: {
    switchTType(e, type) {
      this.ttype = type
    },
  },
}
</script>
<style lang="scss" scoped>
.f-card {
  padding: 30px;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 10px;
  backdrop-filter: blur(2px);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.01);
}
.f-card__inner {
  display: flex;
  align-items: center;
}
.f-card__img-wrap {
  margin: 0;
  min-width: 80px;
  width: 40%;
  display: block;
  align-self: center;
  img {
    width: 100%;
    height: auto;
  }
}
.f-card__info-container {
  padding-left: 30px;
  margin-left: auto;
  color: #fff;
  text-align: right;
}
.f-card__location-name {
  font-size: 24px;
  margin: 0;
  @include _lg {
    font-size: calc(
      14px + (24 - 14) * ((100vw - #{$minS}px) / (#{$maxS} - #{$minS}))
    );
  }
  @include _sm {
    font-size: 14px;
  }
}
.f-card__temperature-grid {
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-rows: repeat(4, 1fr);
  grid-template-areas:
    'temp degree'
    'temp .'
    'temp сelsius'
    'temp fahrenheit';
  column-gap: 5px;
  line-height: 0.75;
  margin-top: 15px;
}
.f-card__temperature {
  font-size: 120px;
  font-weight: 700;
  align-items: start;
  grid-area: temp;
  @include _lg {
    font-size: calc(
      60px + (120 - 80) * ((100vw - #{$minS}px) / (#{$maxS} - #{$minS}))
    );
  }
  @include _sm {
    font-size: 80px;
  }
}
.f-card__temperature-degree {
  grid-area: degree;
  width: 12px;
  height: 12px;
  margin-left: auto;
  background: #fff;
  border-radius: 50%;
}
.f-card__temperature-type {
  font-weight: 700;
  font-size: 18px;
  color: rgba(255, 255, 255, 0.4);
  cursor: pointer;
  text-align: left;
  align-self: end;
  &.active {
    color: #fff;
  }
  &.--сelsius {
    grid-area: сelsius;
  }
  &.--fahrenheit {
    grid-area: fahrenheit;
  }
}
</style>
