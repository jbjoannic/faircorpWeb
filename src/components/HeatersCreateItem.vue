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
        <input v-model="createHeaterName" placeholder="Name">
        <select v-model="createHeaterRoom">
          <option disabled value="">Select Room</option>
          <option v-for="room in rooms" :key="room.id" :value="room.id">{{room.name}}</option>
        </select>
        <input v-model="createHeaterPower" placeholder="Power">
        <select v-model="createHeaterStatus">
          <option disabled value="">Select Status</option>
          <option value="ON">ON</option>
          <option value="OFF">OFF</option>
        </select>
        <button type="button" class="btn btn-secondary me-2" @click="createHeater">Create heater</button>
      </div>
    </template>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
  name: 'HeatersCreateItem',
  data: function() {
    return {
      isExpanded: false,
      rooms:[],
      createHeaterName:"",
      createHeaterRoom:"",
      createHeaterStatus:"",
      createHeaterPower:""
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async createHeater() {
      let response = await axios.post(`${API_HOST}heaters/create`,{"name":`${this.createHeaterName}`,"heaterStatus":`${this.createHeaterStatus}`,"power":`${this.createHeaterPower}` ,"roomId":`${this.createHeaterRoom}`});
      let createdHeater = response.data;
      this.$emit('heater-updated');
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}rooms`);
    let rooms = response.data;
    this.rooms = rooms;
  }
}
</script>

<style lang="scss" scoped>

.open-status {
  .icon {
    position: relative;
  }

  &.on {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }

  &.off {
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