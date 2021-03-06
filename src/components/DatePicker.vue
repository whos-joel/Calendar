<template>
    <div class="date-picker">
        <div>{{monthYear}}</div>
        <div class="dates-container">
            <div class="date" v-for="date in dates" :key="date.id">
                <a href="#" class="" v-if="date.isWithinMonth && date.isWithinRange"
                        :class="{   'selected' : date.isSelected, 
                                    'start' : date.isStart, 
                                    'end' : date.isEnd}"
                        :data-value="date.value"
                        @click="date.isWithinRange ? onClick(date.value) : null"
                        @mouseenter="date.isWithinRange ? onHover(date.value) : null">
                    {{date.date}}
                </a>
                <div v-if="!date.isWithinMonth || !date.isWithinRange"
                     :class="{  'not-within-month' : !date.isWithinMonth,
                                'not-within-range' : !date.isWithinRange}">
                        {{date.date}}
                </div>
            </div>
       </div>
    </div>
</template>

<script lang="ts">

import { Component, Vue, Prop, Emit } from 'vue-property-decorator';
import moment from 'moment'

@Component
export default class DatePicker extends Vue{
    data:Object[] = [];
    //start:number = -1;
    //end:number = -1;

    @Prop()
    highlightOnHover!:boolean;

    @Prop()
    startMode!:boolean;

    @Prop()
    endMode!:boolean;

    @Prop({default: -1})
    start!:number;

    @Prop({default: -1})
    end!:number;

    @Prop({default: new Date().getMonth()})
    public selectedMonth!: number;

    @Prop({default: new Date().getFullYear()})
    public selectedYear!: number;

    @Prop({default: 1})
    public firstDayOfTheWeek!: number;

    @Prop({default: 0})
    public index!: number;

    @Prop()
    public min?: Date;

    @Prop()
    public max?: Date;


    get dates():Object[]{
        let firstDayOfTheMonth = this.getFirstDayOfMonth(this.selectedYear, this.selectedMonth);
        let data:Object[] = [];
        for(let i = 0; i < 42; i++){
            let day = i + 1 - firstDayOfTheMonth;
            let date = this.getDate(day);
            let value = +date;
            data[i] = {
                id:i,
                date: date.getDate(),
                value: value,
                isSelected: this.start > -1 && value > this.start && value < this.end,
                isStart: value === this.start,
                isEnd: value === this.end,
                isWithinMonth: date.getMonth() === this.selectedMonth,
                isWithinRange: this.getWithinRange(value)
            }
        }
        return data;
    }

    get monthYear() {
        return moment(new Date(this.selectedYear, this.selectedMonth)).format('MMM YYYY');
    }

    getWithinRange(date:number):boolean{
        if(this.min && this.max)
            return date >= +this.min && date <= +this.max;
        if(this.min)
            return date >= +this.min;
        if(this.max)
            return date <= +this.max;
        return true;
    }

    getDate(day: number) {
        return new Date(this.selectedYear, this.selectedMonth, day);
    }

    getFirstDayOfMonth(year: number, month: number) {
        return new Date(year, month, 1).getDay();
    }

    @Emit("selected")
    onClick(val:number){
        if(this.startMode){
            this.highlightForStartMode(val);
        }
        else if(this.endMode)
            this.highlightForEndMode(val);
        return val;
    }

    highlightForStartMode(id:number){
         if(this.start > -1 && this.end > -1 && this.highlightOnHover){
             this.updateHighlightOnHover(false);
        }
        else if(this.start > -1 && this.end > -1){
            this.updateStart(id);
            if(this.start > this.end){
                this.updateHighlightOnHover(true);
                this.updateEnd(-1);
            }
        }
        else if(this.start === -1){
            this.updateStart(id);
            this.updateHighlightOnHover(true);
        }
    }

    highlightForEndMode(id:number){
        if(this.start > -1 && this.end > -1 && this.highlightOnHover){
             this.updateHighlightOnHover(false);
        }
        else if(this.start > -1 && this.end > -1){
            this.updateEnd(id);
            if(this.start > this.end){
                this.updateHighlightOnHover(true);
                this.updateStart(-1);
            }
        }
        else if(this.end === -1){
            this.updateEnd(id);
            this.updateHighlightOnHover(true);
        }
    }

    onHover(id:number){
        if(this.startMode){
             if(this.start > -1 && this.highlightOnHover && id > this.start){
                this.updateEnd(id);
            }
        }
        if(this.endMode){
             if(this.end > -1 && this.highlightOnHover && id < this.end){
                this.updateStart(id);
            }
        }
    }

    @Emit("updateStart")
    updateStart(val:number):number{
        return val;
    }

    @Emit("updateEnd")
    updateEnd(val:number):number{
        return val;
    }

    @Emit("highlightOnHover")
    updateHighlightOnHover(val:boolean):boolean{
        return val;
    }
}
</script>

<style lang="less">
    .date-picker{
        .dates-container{
        display: flex;
        width:400px;
        flex-wrap: wrap;
        
            .date{
                width: calc(100% / 7);
                a, > div{
                    text-decoration: none;
                    color: #ccc;
                    background-color: #f7f7f7;
                    display: block;
                    padding: 15px;
                    border:1px solid #fff;
                    
                    &:focus{
                        //border: 2px solid aqua;
                    }
                    &.selected{
                        background-color: #ddd;
                        color: #999;
                    }
                    &.start, &.end{
                        background-color: #ccc;
                        color: #fff;
                    }
                    &.not-within-month{
                         background-color: #fcfcfc;
                         color:#ddd;
                         cursor: not-allowed
                    }
                    &.not-within-range{
                        background-color: #fff;
                        color:#ddd;
                         cursor: not-allowed
                    }
                }
            }
        }
    }
</style>

