<template>
    <div class="calendar">
        <!-- <div class="btn-container">
            <a href="#" class="btn" @click="setStartMode()" :class="{'selected' : startMode}">
                {{getDateStr(start)}}
            </a>
            <a href="#" class="btn" @click="setEndMode()" :class="{'selected' : endMode}">
               {{getDateStr(end)}}
            </a>
        </div> -->
        <div class="btn-container nav-months">
            <a href="#" class="btn" @click="!isAtMin() ? onPreviousMonth() : null" :class="{'disabled' : isAtMin()}">
                &lt;
            </a>
            <a href="#" class="btn" @click="!isAtMax() ? onNextMonth() : null" :class="{'disabled' : isAtMax()}">
               &gt;
            </a>
        </div>
        <div class="date-pickers-container">
             <date-picker v-for="i in numberOfMonths" :key="i"
                :startMode="startMode"
                :endMode="endMode"
                :start="start"
                :end="end"
                :selectedMonth="month + i - 1" 
                :selectedYear="year" 
                :index="0"
                :highlightOnHover="updateHighlightOnHover"
                :min="min"
                :max="max"
                @updateStart="onUpdateStart"
                @updateEnd="onUpdateEnd"
                @highlightOnHover="onUpdateHighlightOnHover"
                @selected="onSelected">
            </date-picker>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop, Emit } from 'vue-property-decorator';
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
    //startMode:boolean = true;
    //endMode:boolean = false;
    reset:boolean = false;
    year: number = 0;
    month: number = 0;
    updateHighlightOnHover:boolean = false;

    //@Prop({default: () => {let t = new Date().setHours(0,0,0,0); return [new Date(t), new Date(t)]}})
    @Prop({default: () => []})
    public value!: Date[];

    @Prop()
    public min?: Date;

    @Prop()
    public max?: Date;

    @Prop({default: 3})
    public numberOfMonths?: number;

    @Prop({default: true})
    public setStartMode!: boolean;

    @Prop({default: false})
    public setEndMode!: boolean;

    get startMode():boolean{
        return this.setStartMode;
    }

    get endMode():boolean{
       return this.setEndMode;
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

    onNextMonth() {
        if(this.isAtMax())
            return;
        if (this.month === 11) {
            this.month = 0;
            this.year ++;
        } else {
            this.month ++;
        }
    }

     onPreviousMonth() {
         if(this.isAtMin())
            return;
        if (this.month === 0) {
            this.month = 11;
            this.year --;
        } else {
            this.month --;
        }
    }

    @Emit("input")
    onSelected(val:number){
        if(this.startMode)
            if(this.value[0] && this.value[1])
                return this.onUpdateStartDate(val)
            else if(this.value[0])
                return this.onUpdateEndDate(val)
            else
                return this.onUpdateStartDate(val)
        if(this.endMode)
            if(this.value[0] && this.value[1])
                return this.onUpdateEndDate(val)
            else if(this.value[1])
                return this.onUpdateStartDate(val)
            else
                return this.onUpdateEndDate(val)
    }

    onUpdateStartDate(val:number){
        return [new Date(val), this.value[1]];
    }

    onUpdateEndDate(val:number){
        return [this.value[0], new Date(val)];
    }

    isAtMax(){
        return this.max && this.month >= this.max.getMonth() && this.year >= this.max.getFullYear();
    }

    isAtMin(){
        return this.min && this.month <= this.min.getMonth() && this.year <= this.min.getFullYear()
    }

    created() {
        let start = this.value[0] ? this.value[0] :new Date(new Date().setHours(0,0,0,0));
       
        this.year = start.getFullYear();
        this.month = start.getMonth();
    }

}
</script>

<style lang="less">
    .calendar{
        width: 100%;
        
        .date-pickers-container{
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        .btn-container{
            display: flex;
            justify-content: space-between;
            padding: 15px 0;
            align-self: flex-start;
            .btn{
                display: block;
                padding: 3px 17px;
                color: #fff;
                background-color: #ccc;
                text-decoration: none;
                
                font-size: 2rem;

                &.selected{
                    background-color: #bbb;
                }

                &.disabled{
                    background-color: #f7f7f7;
                }
            }

            &.nav-months{
                justify-content: space-between;
            }
        }
    }
    
</style>


