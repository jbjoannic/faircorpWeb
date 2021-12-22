<template>
  
  <div class="windows-list pt-3" v-show="show=='4'">
    <buildings-list-item 
      v-for="building in buildings"
      :building="building"
      :key="building.id"  
      @building-updated="updateBuilding"
      @see-rooms="buildingRooms"
    >
    </buildings-list-item>
    <buildings-create-item
    @building-updated="updateBuilding"></buildings-create-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import BuildingsListItem from './BuildingsListItem';
import BuildingsCreateItem from './BuildingsCreateItem.vue';
export default {
  props: {
    show:{required: true, type: String}
  },
  components: {
    BuildingsListItem,
    BuildingsCreateItem
  },
  name: 'BuildingsList',
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      buildings: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}buildings`);
    let buildings = response.data;
    this.buildings = buildings;
    
  },
  methods: {
    updateBuilding(newBuilding) {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      let index = this.buildings.findIndex(building => building.id === newBuilding.id);
      this.buildings.splice(index, 1, newBuilding);
    },
    buildingRooms(buildingId) {
      this.$emit("see-rooms", buildingId);
    },
    updateBuilding() {
      this.$emit("building-updated");
    }
  }
}
</script>
