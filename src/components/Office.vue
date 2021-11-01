<template>
  <div class="office">
    <Map :legend="legend" />
    <SideMenu :legend="legend" />
  </div>
</template>

<script>
import legendData from '../assets/data/legend.json';
import Map from "./Office/Map.vue";
import SideMenu from "./Office/SideMenu.vue";

export default {
  name: 'Office',
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      legend: [],
    };
  },
  created() {
    this.loadLegend();
  },
  methods: {
    loadLegend() {
      this.legend = legendData;
    },
  }
};
</script>

<style scoped>
.office {
    display: grid;
    grid-template-columns: 1fr 320px;
    border-radius: 6px;
    border: 1px solid #ccd8e4;
    height: 100%;
    background: white;
    max-width: 1500px;
    margin: 0 auto;
}
</style>