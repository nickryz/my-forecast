<template>
  <div class="f-card-lay">
    <div class="container">
      <div class="f-card-lay__inner">
        <FCard
          v-for="(card, i) in f_cards"
          :f_card="card"
          :key="card.location + i"
        />
      </div>
    </div>
  </div>
</template>

<script>
import FCard from './FCard'

export default {
  name: 'FCardLay',
  components: { FCard },
  props: {
    f_cards: {
      type: Array,
      default: function () {
        return []
      },
    },
  },
  data() {
    return {}
  },
}
</script>
<style lang="scss" scoped>
.f-card-lay {
  padding: 30px 0;
}
.f-card-lay__inner {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
  gap: 15px;
}
</style>
