<template>
    <div class="filter">
    <span>Filter :</span>
    <select @change="updateBuilding" v-model="buildingFilterId">
        <option disabled value="">Select Building</option>
        <option value="all">AllBuildings</option>
        <option v-for="building in buildings" :key="building.id" :value="building.id">{{building.name}}</option>
    </select>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
    name: 'BuildingFilter',
    data: function() {
        return {
            buildings:[],
            buildingFilterId:""
        }
    },
    created: async function() {
        let response = await axios.get(`${API_HOST}buildings`);
        let buildings = response.data;
        this.buildings = buildings;
    },
    methods: {
        updateBuilding(){
            this.$emit('building-filtered', this.buildingFilterId);
        }
    }
    
}
</script>
<style scoped>
.filter{
    display: -webkit-inline-flex;
    align-items:center;
}
</style>