<template>
  
  <div class="windows-list pt-3" v-show="show=='1'">
    <p>ID{{roomFilterIdProp}}</p>
    <room-filter
        @room-filtered="updateFilterRoom"></room-filter>
    <windows-list-item 
      v-for="window in windows"
      :window="window"
      :key="window.id"  
      @window-updated="updateWindow"
    >
    </windows-list-item>
    <windows-create-item
    @window-updated="updateWindow"></windows-create-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomFilter from './RoomFilter.vue';
import WindowsListItem from './WindowsListItem';
import WindowsCreateItem from './WindowsCreateItem.vue';
export default {
  props: {
    show:{required: true, type: String},
    roomFilterIdProp:{type:String}
  },
  components: {
    WindowsListItem,
    WindowsCreateItem,
    RoomFilter
  },
  name: 'WindowsList',
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      windows: [],
      roomId:'',
      roomFilterId:'',
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}windows`);
    let windows = response.data;
    this.windows = windows;
    this.roomFilterId=this.roomFilterIdProp;

    
  },
  watch : {
    roomFilterId : "updateFilterRoomProp"
  },
  methods: {
    updateWindow() {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      this.$emit('window-updated');
    },
    async updateFilterRoom(newRoom) {
      this.roomId=newRoom;
      if (this.roomId=="all") {
      let response = await axios.get(`${API_HOST}windows`);
      let windows = response.data;
      this.windows = windows;
      } else {
        let response = await axios.get(`${API_HOST}rooms/${this.roomId}/windows`);
        let windows = response.data;
        this.windows = windows;
      }
    },
    async updateFilterRoomProp() {
      this.roomId=this.roomFilterId;
      if (this.roomId=='') {
      } else if (this.roomId=="all") {
      let response = await axios.get(`${API_HOST}windows`);
      let windows = response.data;
      this.windows = windows;
      } else {
        let response = await axios.get(`${API_HOST}rooms/${this.roomId}/windows`);
        let windows = response.data;
        this.windows = windows;
      }
    }
  }
}
</script>
