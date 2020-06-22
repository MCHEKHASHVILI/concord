<style scopped>
.demo-list-wrap {
  width: 100%;
  max-width: 360px;
  overflow:hidden;
}
</style>
<template>
<mu-drawer :open.sync="open" v-elevation="4" :docked="docked" :right="position === 'right'">
  <mu-appbar z-depth="0" color="#fff" textColor="rgba(0,0,0,.54)">
    <a href="/project">Concord Travel</a>
  </mu-appbar>
  <hr class="mu-divider">
    <mu-list toggle-nested>
    <mu-list-item button textline="one-line"
      :ripple="false" 
      nested :open="open === 'menuItem.title'" 
      @toggle-nested="open = arguments[0] ? menuItem.title : ''"
      v-bind:key="menuItem + index"
      v-for="(menuItem,index) in sidebar.list">
      <mu-list-item-action>
        <mu-icon :value="menuItem.icon"></mu-icon>
      </mu-list-item-action>
      <mu-list-item-title>{{menuItem.name}}</mu-list-item-title>
      <mu-list-item-action>
        <mu-icon class="toggle-icon" size="24" value="keyboard_arrow_down"></mu-icon>
      </mu-list-item-action>
      <mu-list-item button :ripple="false" slot="nested" v-bind:key="item + i" v-for="(item,i) in menuItem.items">
        <mu-list-item-title><a :href="item.link">{{item.name}}</a></mu-list-item-title>
      </mu-list-item>
    </mu-list-item>
  </mu-list>
  </mu-drawer>
</template>
<script>
    import sideBar from '../../components/data/sidebar';
    export default {
        name: 'sidebar',
        data: function(){
            return {
                docked: true,
                open: true,
                position: 'left',
                sidebar: sideBar
            }
        }
    }
</script>