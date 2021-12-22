<template>
  <div class="window border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}"  @mouseover="hover=true" @mouseleave="hover=false">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="window-name fw-bold pe-3">{{room.name}}</div>
      <div class="room-name text-muted">{{room.buildingName}}</div>
        <div class="room-name text-muted" v-if="hover">Floor:{{room.floor}}</div>
        <div class="room-name text-muted" v-if="hover">CurrentTemp:{{room.currentTemperature}}</div>
        <div class="room-name text-muted" v-if="hover">TargetTemp:{{room.targetTemperature}}</div>
      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <button type="button" class="btn btn-secondary me-2" @click="roomWindows">See windows</button>
        <button type="button" class="btn btn-secondary me-2" @click="roomHeaters">See heaters</button>
        <button type="button" class="btn btn-danger" @click="deleteRoom">Delete Room</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'RoomsListItem',
  props: ['room'],
  data: function() {
    return {
      isExpanded: false,
      hover: false
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async deleteRoom() {
      await axios.delete(`${API_HOST}rooms/${this.room.id}/delete`);
      this.$emit("room-updated");
    },
    roomWindows() {
      this.$emit("see-windows",this.room.id);
    },
    roomHeaters() {
      this.$emit("see-heaters",this.room.id);
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
.top-row {
  overflow: auto;
}
.room-name{
  margin-right: 8px;
}
</style>
