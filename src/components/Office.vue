<template>
  <div
    class="office"
    @click="officeClickHandler"
  >
    <Map :legend="legend" />
    <SideMenu
      :legend.sync="legend"
      :person.sync="personAtWorkplace"
      :isUserOpenned.sync="isWorkplaceSelected"
    />
  </div>
</template>

<script>
import legendData from '../assets/data/legend.json';
import peopleData from '../assets/data/people.json'
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
      legend: null,
      people: null,
      isWorkplaceSelected: false,
      personAtWorkplace: null,
    };
  },
  created() {
    this.loadLegend();
    this.loadPeople();
  },
  methods: {
    loadLegend() {
      this.legend = legendData;
    },
    loadPeople() {
      this.people = peopleData;
    },
    getPersonAtWorkplace(wpId) {
      return this.people.find((item) => item.tableId === wpId);
    },
    officeClickHandler(event) {
      const clickedWpElement = event.target.closest('.workplace');
      if (clickedWpElement) {
        const clickedWpId = Number(clickedWpElement.getAttribute('id').match(/\d+/));
        this.personAtWorkplace = this.getPersonAtWorkplace(clickedWpId);
        this.isWorkplaceSelected = true;
      }
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