<style scopped>

</style>
<template>
  <div>
    <side-bar></side-bar>

    <platform></platform>
    
  </div>
</template>
<script>
  import sidebar from '../../project/menu/sidebar.vue'
  import platform from '../../project/platform/platform.vue'

    export default {
        name: 'topnav',
        components:{
          'side-bar': sidebar,
          'platform': platform
        },
        data: function(){
            return {
                send: 'open'
            }
        }
    }
</script>