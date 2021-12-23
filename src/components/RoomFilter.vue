<template>
    <div class=filter>
    <span class=text-filter>Filter :</span>
    <select @change="updateRoom" v-model="roomFilterId">
        <option disabled value="">Select Room</option>
        <option value="all">AllRooms</option>
        <option v-for="room in rooms" :key="room.id" :value="room.id">{{room.name}}</option>
    </select>
    </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
export default {
    name: 'RoomFilter',
    data: function() {
        return {
            rooms:[],
            roomFilterId:''
        }
    },
    
    created: async function() {
        let response = await axios.get(`${API_HOST}rooms`);
        let rooms = response.data;
        this.rooms = rooms;
    },
    methods: {
        updateRoom(){
            this.$emit('room-filtered', this.roomFilterId);
        }
    }
    
}
</script>
<style scoped>
.filter{
    display: -webkit-inline-flex;
    align-items:center;
}
</style>