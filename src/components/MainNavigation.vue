<template>
  <div>
  <ul class="nav nav-tabs">
    <li class="nav-item">
      <a :class=activeClass1 aria-current="page" href="#" @click="click1">Windows</a>
    </li>
    <li class="nav-item">
      <a :class=activeClass2 href="#" aria-disabled="true" @click="click2">Heathers</a>
    </li>
    <li class="nav-item">
      <a :class=activeClass3 href="#" aria-disabled="true" @click="click3">Rooms</a>
    </li>
    <li class="nav-item">
      <a :class=activeClass4 href="#" aria-disabled="true" @click="click4">Buildings</a>
    </li>
  </ul>
  </div>
</template>


<script>
export default {
  name: 'MainNavigation',
  data: function() {
    return {
      active: "1"
    }
  },
  computed: { 
    activeClass1: function() {
      return this.active=='1' ? 'nav-link active' : 'nav-link'
    },
    activeClass2: function() {
      return this.active=='2' ? 'nav-link active' : 'nav-link'
    },
    activeClass3: function() {
      return this.active=='3' ? 'nav-link active' : 'nav-link'
    },
    activeClass4: function() {
      return this.active=='4' ? 'nav-link active' : 'nav-link'
    }
  },
  
  methods: {
    click1() {
      this.$emit('nav-updated', "1");
      this.active='1';
    },
    click2() {
      this.$emit('nav-updated', "2");
      this.active='2';
    },
    click3() {
      this.$emit('nav-updated', "3");
      this.active='3';
    },
    click4() {
      this.$emit('nav-updated', "4");
      this.active='4';
    }
  }
}
</script>
