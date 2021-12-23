<template>
  <div class="window border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
  <div class="top-row d-flex" @click="toggleExpand">
      <div class="window-name fw-bold pe-3">Create</div>
      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <input v-model="createBuildingName" placeholder="Name">
        <input v-model="createBuildingOutsideTemperature" placeholder="Outside Temperature">
        <button type="button" class="btn btn-secondary me-2" @click="createBuilding">Create building</button>
      </div>
    </template>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
  name: 'BuildingsCreateItem',
  data: function() {
    return {
      isExpanded: false,
      rooms:[],
      createBuildingName:"",
      createBuildingOutsideTemperature:""
      
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async createBuilding() {
      let response = await axios.post(`${API_HOST}buildings/create`,{"name":`${this.createBuildingName}`,"outsideTemperature":`${this.createBuildingOutsideTemperature}`});
      let createdBuilding = response.data;
      this.$emit('building-updated');
    }
  }
}
</script>

<style lang="scss" scoped>

.open-status {
  .icon {
    position: relative;
  }

  &.open {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }

  &.closed {
    color: #dc3545;
  }
}
.btn-danger{
  margin-right: 8px;
}
.window {
  .top-row {
    cursor: pointer;
  }
}
.details{
  overflow: auto;
}
.window{
  background-color: green;
}
.btn-secondary{
  background-color: rgba(221, 255, 169, 0.63);
}
</style>