<style scoped>
.mu-tabs{
    overflow-x: scroll;
    z-index: 0;
}
.d-flex{
    margin: 0;
    
}
</style>
<template>
    <div>
        <mu-tabs :value.sync="active" color="grey500">
            <mu-tab v-bind:key="day + index" v-for="(day,index) in days">
                Day {{(index + 1)}}
            </mu-tab>
        </mu-tabs>
        
        <div v-bind:key="day + index" v-for="(day,index) in days">
            <mu-flex class="flex-wrapper" align-items="start" style="width:100%" v-if="active === index">
                <mu-flex class="flex-demo" justify-content="center" fill>
                    <mu-paper class="demo-paper" :z-depth="1" style="width:100%">
                        <div class="demo-container" style="width:100%; padding: 5px;">
                            Details of Day {{(index + 1)}}
                        </div>
                    </mu-paper>
                </mu-flex>
            </mu-flex>
        </div>
    </div>
</template>
<script>
export default {
    name: 'detailed',
    props:['days'],
    data: function(){
        return {
            active: 0,
            data: []
        }
    },
    computed: {
        dragOptions: function() {
            return {
                animation: 500,
                group: "description",
                disabled: false,
                ghostClass: "ghost"
            };
        },
    },
    methods:{
        //
    }
}
</script>