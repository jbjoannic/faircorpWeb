<template>
  <div class="window border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}"  @mouseover="hover=true" @mouseleave="hover=false">
    <div class="top-row d-flex" @click="toggleExpand" @mouseover="hover=true" @mouseleave="hover=false">
      <div class="window-name fw-bold pe-3">{{building.name}}</div>
      <div class="room-name text-muted" v-if="hover">Outside Temperature:{{building.outsideTemperature}}</div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <button type="button" class="btn btn-secondary me-2" @click="buildingRooms">See Rooms</button>
        <button type="button" class="btn btn-danger" @click="deleteBuilding">Delete building</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'BuildingsListItem',
  props: ['building'],
  data: function() {
    return {
      isExpanded: false,
      hover:false
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    buildingRooms() {
      this.$emit("see-rooms", this.building.id);
    },
    async deleteBuilding() {
      await axios.delete(`${API_HOST}buildings/${this.building.id}/delete`);
      this.$emit("building-updated");
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
.btn-secondary {
  background-color: green;
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
