<template>
  <div id="app">
    <header>
      <div class="title">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto" :key="componentKey">
      <main-navigation
        @nav-updated="updateNav"
        :activeAskedProp="valShow"></main-navigation>
      <windows-list :show="valShow" :roomFilterIdProp="valRoom"
      @window-updated=update></windows-list>
      <heaters-list :show="valShow" :roomFilterIdProp="valRoom"
      @heater-updated=update></heaters-list>
      <rooms-list :show="valShow"
      @see-windows=roomWindows
      @see-heaters=roomHeaters
      @room-updated=update
      :buildingFilterIdProp="valBuilding"></rooms-list>
      <buildings-list :show="valShow"
      @see-rooms=buildingRooms
      @building-updated=update></buildings-list>
    </section>
  </div>
</template>

<script>
import BuildingsList from './components/BuildingsList.vue';
import HeatersList from './components/HeatersList.vue';
import MainNavigation from './components/MainNavigation.vue';
import RoomsList from './components/RoomsList.vue';
import WindowsList from './components/WindowsList.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    WindowsList,
    HeatersList,
    RoomsList,
    BuildingsList,
  },
  data: function() {
    return {
      title: 'Faircorp : Manage buildings elements',
      valShow: '1',
      valRoom: '',
      valBuilding:'',
      componentKey: 0
    }
  },
  methods: {
    updateNav(newShow) {
      this.valShow=newShow;
    },
    roomWindows(roomId) {
      this.valShow='1';
      this.valRoom=`${roomId}`;
      this.componentKey++;
    },
    roomHeaters(roomId) {
      this.valShow='2';
      this.valRoom=`${roomId}`;
      this.componentKey++;
    },
    buildingRooms(buildingId) {
      this.valShow="3";
      this.valBuilding=`${buildingId}`
      this.componentKey++;
    },
    update() {
      this.componentKey++;
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 60px;
  background-color: rgba(221, 255, 169, 0.63);;

}
.title {
  font-size: 40px;
}
</style>
