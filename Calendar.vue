<template>
    <div class="calendar">
        <div class="btn-container">
            <a href="#" class="btn" @click="setStartMode(true)" :class="{'selected' : startMode}">
                {{start}}
            </a>
            <a href="#" class="btn" @click="setEndMode(true)" :class="{'selected' : endMode}">
               {{end}}
            </a>
        </div>
        <date-picker
            :startMode="startMode"
            :endMode="endMode"
            :start="start"
            :end="end"
            @updateStart="onUpdateStart"
            @updateEnd="onUpdateEnd">
        </date-picker>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import DatePicker from './DatePicker.vue';

@Component({
    components: {
        DatePicker
    }
})
export default class Calendar extends Vue{
    
    start:number = -1;
    end:number = -1;
    startMode:boolean = false;
    endMode:boolean = false;
    reset:boolean = false;

    setStartMode(value:boolean){
        this.startMode = value;
        this.endMode = !value;
    }

    setEndMode(value:boolean){
        this.endMode = value;
        this.startMode = !value;
    }

    onUpdateStart(val:number){
        this.start = val;
    }

    onUpdateEnd(val:number){
        this.end = val;
    }

}
</script>

<style lang="less">
    .calendar{
        width: 400px;
        .btn-container{
            display: flex;
            justify-content: space-around;
            padding: 15px;
            .btn{
                display: block;
                padding: 15px;
                color: #fff;
                background-color: #ccc;
                text-decoration: none;

                &.selected{
                    background-color: #ccc;
                }
            }
        }
    }
    
</style>


