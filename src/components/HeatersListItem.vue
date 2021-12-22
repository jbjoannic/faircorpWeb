<template>
  <div class="window border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand" @mouseover="hover=true" @mouseleave="hover=false">
      <div class="window-name fw-bold pe-3">{{heater.name}}</div>
      <div class="room-name text-muted">{{heater.roomName}}</div>
      <div class="room-name text-muted" v-if="hover">Power:{{heater.power}}</div>
      <div class="open-status ms-4" :class="{on: isHeaterOn, off: !isHeaterOn}">
        <template v-if="isHeaterOn">
          <span class="icon">&#x2B24;</span> ON
        </template>
        <template v-else>
          <span class="icon">&#x2716;</span> OFF
        </template>
      </div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <button type="button" class="btn btn-secondary me-2" @click="switchHeater">{{ isHeaterOn ? 'OFF' : 'ON' }} Heater</button>
        <button type="button" class="btn btn-danger" @click="deleteHeater">Delete heater</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'HeatersListItem',
  props: ['heater'],
  data: function() {
    return {
      isExpanded: false,
      hover:false
    }
  }, 
  computed: {
    isHeaterOn: function() {
      return this.heater.heaterStatus === 'ON'; 
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchHeater() {
      let response = await axios.put(`${API_HOST}heaters/${this.heater.id}/switch`);
      let updatedHeater = response.data;
      this.$emit('heater-updated');
    },
    async deleteHeater() {
      await axios.delete(`${API_HOST}heaters/${this.heater.id}/delete`);
      this.$emit('heater-updated');
    }
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
.room-name{
  margin-right: 8px;
}
</style>
