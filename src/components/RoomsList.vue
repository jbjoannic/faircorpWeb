<template>
  
  <div class="windows-list pt-3" v-show="show=='3'">
    <building-filter
        @building-filtered="updateFilterBuilding"></building-filter>
    <rooms-list-item 
      v-for="room in rooms"
      :room="room"
      :key="room.id"  
      @room-updated="updateRoom"
        @see-windows="roomWindows"
        @see-heaters="roomHeaters"
    >
    </rooms-list-item>
    <rooms-create-item
    @room-updated="updateRoom"></rooms-create-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import BuildingFilter from './BuildingFilter.vue';
import RoomsListItem from './RoomsListItem';
import RoomsCreateItem from './RoomsCreateItem.vue';
export default {
  props: {
    show:{required: true, type: String},
    buildingFilterIdProp:{type:String}
  },
  components: {
    RoomsListItem,
    RoomsCreateItem,
    BuildingFilter
  },
  name: 'RoomsList',
  data: function() {
    return {
      /* Initialize rooms with an empty array, while waiting for actual data to be retrieved from the API */
      rooms: [],
      buildingId: '',
      buildingFilterId:'',
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}rooms`);
    let rooms = response.data;
    this.rooms = rooms;
    this.buildingFilterId=this.buildingFilterIdProp;
    
  },
  watch : {
    buildingFilterId:"updateFilterBuildingProp"
  },
  methods: {
    updateRoom(newRoom) {
      this.$emit("room-updated");
    },
    async updateFilterBuilding(newBuilding) {
        this.buildingId=newBuilding;
        if (this.buildingId=="all") {
            let response = await axios.get(`${API_HOST}rooms`);
            let rooms = response.data;
            this.rooms = rooms;
        } else {
            let response = await axios.get(`${API_HOST}buildings/${this.buildingId}/rooms`);
            let rooms = response.data;
            this.rooms = rooms;
        }
    },
    roomWindows(roomId){
      this.$emit("see-windows", roomId);
    },
    roomHeaters(roomId){
      this.$emit("see-heaters", roomId);
    },
    async updateFilterBuildingProp() {
      this.buildingId=this.buildingFilterId;
      if(this.buildingId=='') {
      } else if (this.buildingId == 'all') {
        let response = await axios.get(`${API_HOST}rooms`);
        let rooms = response.data;
        this.rooms = rooms;
      } else {
        let response = await axios.get(`${API_HOST}buildings/${this.buildingId}/rooms`);
        let rooms = response.data;
        this.rooms = rooms;
      }
    }
  }
}
</script>
