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
        <input v-model="createWindowName" placeholder="Name">
        <select v-model="createWindowRoom">
          <option disabled value="">Select Room</option>
          <option v-for="room in rooms" :key="room.id" :value="room.id">{{room.name}}</option>
        </select>
        <select v-model="createWindowStatus">
          <option disabled value="">Select Status</option>
          <option value="OPEN">OPEN</option>
          <option value="CLOSED">CLOSED</option>
        </select>
        <button type="button" class="btn btn-secondary me-2" @click="createWindow">Create window</button>
      </div>
    </template>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
  name: 'WindowsCreateItem',
  data: function() {
    return {
      isExpanded: false,
      rooms:[],
      createWindowName:"",
      createWindowRoom:"",
      createWindowStatus:""
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async createWindow() {
      let response = await axios.post(`${API_HOST}windows/create`,{"name":`${this.createWindowName}`,"windowStatus":`${this.createWindowStatus}`, "roomId":`${this.createWindowRoom}`});
      let createdWindow = response.data;
      this.$emit('window-updated');
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
</style>