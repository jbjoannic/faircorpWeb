<template>
  
  <div class="windows-list pt-3" v-show="show=='2'">
    <room-filter
        @room-filtered="updateFilterRoom"></room-filter>
    <heaters-list-item 
      v-for="heater in heaters"
      :heater="heater"
      :key="heater.id"  
      @heater-updated="updateHeater"
    >
    </heaters-list-item>
    <heaters-create-item
    @heater-updated="updateHeater"></heaters-create-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomFilter from './RoomFilter.vue';
import HeatersListItem from './HeatersListItem';
import HeatersCreateItem from './HeatersCreateItem.vue';
export default {
  props: {
    show:{required: true, type: String},
    roomFilterIdProp:{type:String}
  },
  components: {
    HeatersListItem,
    HeatersCreateItem,
    RoomFilter
  },
  name: 'HeatersList',
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      heaters: [],
      roomId: '',
      roomFilterId:'',
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}heaters`);
    let heaters = response.data;
    this.heaters = heaters;
    this.roomFilterId=this.roomFilterIdProp;
    
  },
  watch : {
    roomFilterId : "updateFilterRoomProp"
  },
  methods: {
    updateHeater() {
      this.$emit("heater-updated");
    },
    async updateFilterRoom(newRoom) {
      this.roomId=newRoom;
      if (this.roomId=="all") {
        let response = await axios.get(`${API_HOST}heaters`);
        let heaters = response.data;
        this.heaters = heaters;
      } else {
        let response = await axios.get(`${API_HOST}rooms/${this.roomId}/heaters`);
        let heaters = response.data;
        this.heaters = heaters;
      }
    },
    async updateFilterRoomProp() {
      this.roomId=this.roomFilterId;
      if (this.roomId=="all") {
      let response = await axios.get(`${API_HOST}heaters`);
      let heaters = response.data;
      this.heaters = heaters;
      } else {
        let response = await axios.get(`${API_HOST}rooms/${this.roomId}/heaters`);
        let heaters = response.data;
        this.heaters = heaters;
      }
    }
  }
}
</script>
