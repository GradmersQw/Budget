<template>
  <div class="total-value">Balance: <span :style="{ color: colorTotalBalance  }">{{ total }}</span></div>
</template>

<script>
export default {
  name: "TotalBalance",
  props: {
    total: {
      type: Number,
      default: 0
    }
  },
  data: () => ({

  }),
  computed: {
    colorTotalBalance(){
        switch(true) {
          case this.total > 0:
            return 'green'

          case this.total < 0:
            return 'red'

          default:
            return 'black'
      }
    }
  },
};
</script>

<style scoped>
.total-value {
  font-size: 26px;
  text-transform: uppercase;
  padding: 20px;
  text-align: center;
}
</style>