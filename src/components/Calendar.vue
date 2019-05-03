<template>
    <div class="calendar">
        <div class="btn-container">
            <a href="#" class="btn" @click="setStartMode()" :class="{'selected' : startMode}">
                {{getDateStr(start)}}
            </a>
            <a href="#" class="btn" @click="setEndMode()" :class="{'selected' : endMode}">
               {{getDateStr(end)}}
            </a>
        </div>
        <date-picker v-for="i in 3" :key="i"
            :startMode="startMode"
            :endMode="endMode"
            :start="start"
            :end="end"
            @updateStart="onUpdateStart"
            @updateEnd="onUpdateEnd"
            :selectedMonth="month + (i - 1)" 
            :selectedYear="year" 
            :index="0"
            @highlightOnHover="onUpdateHighlightOnHover"
            :highlightOnHover="updateHighlightOnHover">
        </date-picker>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import DatePicker from './DatePicker.vue';
import moment from 'moment'

@Component({
    components: {
        DatePicker
    }
})
export default class Calendar extends Vue{
    
    start:number = -1;
    end:number = -1;
    startMode:boolean = true;
    endMode:boolean = false;
    reset:boolean = false;
    year: number = 0;
    month: number = 0;
    updateHighlightOnHover:boolean = false;

    @Prop({default: () => new Date()})
    public value!: Date;

    setStartMode(){
        this.startMode = !this.startMode;
        this.endMode = !this.endMode;
    }

    setEndMode(){
        this.startMode = !this.startMode;
        this.endMode = !this.endMode;
    }

    onUpdateStart(val:number){
        this.start = val;
    }

    onUpdateEnd(val:number){
        this.end = val;
    }

    onUpdateHighlightOnHover(val:boolean){
        this.updateHighlightOnHover = val;
    }

    getDateStr(date:number){
        var d = new Date(date);
        return moment(d).format("DD/MM/YYYY");
    }

    created() {
        this.year = this.value.getFullYear();
        this.month = this.value.getMonth();
    }

}
</script>

<style lang="less">
    .calendar{
        width: 100%;
        display: flex;
        justify-content: space-evenly;
        .btn-container{
            display: flex;
            justify-content: space-around;
            padding: 15px;
            align-self: flex-start;
            .btn{
                display: block;
                padding: 15px;
                color: #fff;
                background-color: #ccc;
                text-decoration: none;

                &.selected{
                    background-color: #bbb;
                }
            }
        }
    }
    
</style>


