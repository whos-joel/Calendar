<template>
  <div id="app">
    <div>
      <input type="text" v-model="startDate" 
              placeholder="Start Date" 
              @click="startMode = true"
              :class="{'selected' : startMode}" />
       <input type="text" v-model="endDate" 
              placeholder="End Date" 
              @click="startMode = false"
              :class="{'selected' : !startMode}" />
    </div>
     <calendar v-model=dates
                :min="new Date(2018,3,5)"
                :max="new Date(2020,5,22)"
                :setStartMode="startMode"
                :setEndMode="!startMode"
                :number-of-months="12"></calendar>
    <!-- <year-picker v-model="year"></year-picker> -->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Calendar from './components/Calendar.vue';
import YearPicker from './components/year-picker.vue';
import moment from 'moment';

@Component({
  components: {
    YearPicker,
    Calendar
  },
})
export default class App extends Vue {
  year:number = 2019;
  dates?: Date[] = [];
  startMode:boolean = true;
  

  get startDate(){
    return this.dates && this.dates[0] ? moment(this.dates[0]).format("DD MMM YYYY") : null;
  }

  get endDate(){
    return this.dates && this.dates[1] ? moment(this.dates[1]).format("DD MMM YYYY") : null;
  }

  created(){
    let today = new Date(new Date().setHours(0,0,0,0));

    //this.dates = [today, today];
  }
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
  input{
    padding: 10px;
    margin-right: 20px;
    &.selected, &:focus{
      border: 2px solid #1998e2
    }
  }
  *{
    box-sizing: border-box;
  }
}
</style>
