<template>
  <button
    :class="[
      isChartEnabled ? 'text-chart-active' : 'text-chart-inactive',
      'px-2 py-4 hidden md:flex flex-none items-center border-b-2 mt-2px border-transparent hover:border-theme-accents transition',
    ]"
    @click="toggleChart()"
  >
    <SvgIcon name="chart" view-box="0 0 18 17" />
  </button>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class ToggleChart extends Vue {
  get isChartEnabled(): boolean {
    return this.$store.getters["ui/priceChartOptions"].enabled;
  }

  private toggleChart(): void {
    this.$store.dispatch("ui/setPriceChartOption", { option: "enabled", value: !this.isChartEnabled });
  }
}
</script>

<style scoped>
button.text-chart-active svg {
  color: var(--color-theme-accents);
}
button.text-chart-inactive svg {
  color: var(--color-svg-icon);
}
button:hover svg {
  color: var(--color-theme-accents);
}
</style>
