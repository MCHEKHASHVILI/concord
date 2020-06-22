<style scoped>
</style>
<template>
    <v-select 
        class=""
        :options="data"
        >
        <option v-bind:key="item.id" test="test" v-for="item in data">{{item}}</option>
    </v-select>
</template>
<script>
    import vSelect from 'vue-select'
    import 'vue-select/dist/vue-select.css';
    export default{
        data: function(){
            return {
                //
            }
        },
        components:{
            'v-select': vSelect
        },
        props:['data'],
    }
</script>
