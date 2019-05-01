<template>
    <div class="date-picker">
       <div class="date" v-for="date in dates" :key="date.id">
           <a href="#" 
                :class="{'selected' : date.isSelected, 'start' : date.isStart, 'end' : date.isEnd}"
                @click="onClick(date.id)"
                @mouseenter="onHover(date.id)">
               {{date.value}}
            </a>
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
    highlightOnHover:boolean = false;

    @Prop()
    startMode:boolean;

    @Prop()
    endMode:boolean;

    @Prop({default: -1})
    start:number;

    @Prop({default: -1})
    end:number;

    get dates():Object[]{
        let data:Object[] = [];
        for(let i = 0; i < 42; i++){
            data[i] = {
                id:i,
                value:i,
                isSelected: this.start > -1 && i > this.start && i < this.end,
                isStart: i === this.start,
                isEnd: i === this.end
            }
        }
        return data;
    }

    onClick(id:number){
        if(this.startMode)
            this.highlightForStartMode(id);
        else if(this.endMode)
            this.highlightForEndMode(id);
    }

    highlightForStartMode(id:number){
         if(this.start > -1 && this.end > -1 && this.highlightOnHover){
             this.highlightOnHover = false;
        }
        else if(this.start > -1 && this.end > -1){
             this.updateStart(id);
            if(this.start > this.end){
                this.highlightOnHover = true;
                 this.updateEnd(-1);
            }
        }
        else if(this.start === -1){
            this.updateStart(id);
            this.highlightOnHover = true;
        }
    }

    highlightForEndMode(id:number){
        if(this.start > -1 && this.end > -1 && this.highlightOnHover){
             this.highlightOnHover = false;
        }
        else if(this.start > -1 && this.end > -1){
            this.updateEnd(id);
            if(this.start > this.end){
                this.highlightOnHover = true;
                this.updateStart(-1);
            }
        }
        else if(this.end === -1){
            this.updateEnd(id);
            this.highlightOnHover = true;
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
                this.updateEnd(id);
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
}
</script>

<style lang="less">
    .date-picker{
        display: flex;
        width:100%;
        flex-wrap: wrap;
            .date{
                width: calc(100% / 7);
                a{
                    text-decoration: none;
                    color: #ccc;
                    background-color: #f7f7f7;
                    display: block;
                    padding: 15px;
                    border:1px solid #fff;
                    &:focus{
                        border-color: aqua;
                    }
                    &.selected{
                        background-color: #ddd;
                    }
                    &.start, &.end{
                        background-color: #ccc;
                    }
                }
        }
    }
</style>

