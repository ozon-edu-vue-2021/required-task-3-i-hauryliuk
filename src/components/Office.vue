<template>
  <div
    class="office"
    @click="officeClickHandler"
  >
    <Map :legend="legend" />
    <SideMenu
      :legend.sync="legend"
      :person.sync="selectedPerson"
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
      selectedPerson: null,
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
      if (event.target.closest('.workplace')) {
        const clickedWpId = Number(event.target.closest('.workplace')
          .getAttribute('id').match(/\d+/));
        const personAtWorkplace = this.getPersonAtWorkplace(clickedWpId);
        if (personAtWorkplace) {
          this.selectedPerson = personAtWorkplace;
        } else {
          this.selectedPerson = null;
        }
        this.isWorkplaceSelected = true;
      } else if (event.target.closest('.map')) {
        this.selectedPerson = null;
        this.isWorkplaceSelected = false;
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