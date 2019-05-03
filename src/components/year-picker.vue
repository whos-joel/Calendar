<template>
    <div class="year-picker">
        <table>
            <tr v-for="(row, i) in years"
                :key="i">
                    <td v-for="year in row"
                        :key="year.id">
                        <a href="#"
                            :class="{'selected' : year.selected}"
                            @click="setYear(year.value)">
                            {{year.value}}
                        </a>
                    </td>
                </tr>
        </table>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import moment from 'moment'

export default Vue.extend({
    name: 'year-picker',

    data: function(){
        return {
            selectedYear: 2019,
        }
    },

    props:{
        value:Number
    },

    computed:{
        years: function(){
            let data = [];
            let start = this.value;
            let n = 0;

            for(var i = 0; i < 4; i++){
                data[i] = [];
                for(var j = 0; j < 4; j++){
                    data[i][j] = {
                        id:n,
                        selected: start == this.value,
                        value: start++,
                    }
                    n++;
                }
            }
            
            return data;
        }
    },

    methods:{
         setYear:function(val){
           this.$emit("input", val)
        },
    }
})
</script>

<style lang="less">
    .year-picker{
        display: flex;

        table{
            td{
                a{
                    text-decoration: none;
                    color: #f7f7f7;
                    background-color: #f7f7f7;
                    display: block;
                    padding: 15px;
                    border:1px solid grey;
                    &:focus{
                        border-color: aqua;
                    }
                    &.selected{
                        background-color: #ddd;
                    }
                }
            }
        }
    }
</style>



