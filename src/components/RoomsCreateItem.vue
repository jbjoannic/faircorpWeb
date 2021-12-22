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
        <input v-model="createRoomName" placeholder="Name">
        <select v-model="createRoomBuilding">
          <option disabled value="">Select building</option>
          <option v-for="building in buildings" :key="building.id" :value="building.id">{{building.name}}</option>
        </select>
        <input v-model="createRoomFloor" placeholder="floor">
        <input v-model="createRoomCurrentTemperature" placeholder="Current temperature">
        <input v-model="createRoomTargetTemperature" placeholder="Target Temperature">
        <button type="button" class="btn btn-secondary me-2" @click="createRoom">Create room</button>
      </div>
    </template>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
  name: 'RoomsCreateItem',
  data: function() {
    return {
      isExpanded: false,
      buildings:[],
      createRoomName:"",
      createRoomBuilding:"",
      createRoomFloor:"",
      createRoomCurrentTemperature:"",
      createRoomTargetTemperature:""
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async createRoom() {
      let response = await axios.post(`${API_HOST}rooms/create`,
      {"name":`${this.createRoomName}`,
      "buildingId":`${this.createRoomBuilding}`,
      "floor":`${this.createRoomFloor}`,
      "currentTemperature":`${this.createRoomCurrentTemperature}`,
      "targetTemperature":`${this.createRoomTargetTemperature}`});
      let createdRoom = response.data;
      this.$emit('room-updated');
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}buildings`);
    let buildings = response.data;
    this.buildings = buildings;
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
</style>