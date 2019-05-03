<template>
  <div id="app">
     <calendar v-model=date></calendar>
    <!-- <year-picker v-model="year"></year-picker> -->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Calendar from './components/Calendar.vue';
import YearPicker from './components/year-picker.vue';

@Component({
  components: {
    YearPicker,
    Calendar
  },
})
export default class App extends Vue {
   year:number = 2019
   dates: Date = new Date();
}
</script>

<style lang="less">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
