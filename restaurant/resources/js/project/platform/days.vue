<style scoped>
.md-content{
    height: 305px;
    overflow: scroll;
}
</style>
<template>
  <mu-row gutter>
    <md-content class="md-scrollbar" style="width:100%">
      <draggable v-if="data.length > 0" v-model="data" v-bind="dragOptions" handle=".handle">
        <transition-group>
          <div v-bind:key="index + '-' + item" v-for="(item,index) in data">
            <md-card>
              <md-toolbar class="md-secondary">
                <mu-button fab small color="secondary" class="handle">
                  <md-icon>menu</md-icon>
                </mu-button>
                <span
                  class="md-title"
                >Day {{ item.day }}</span>
                <div class="md-toolbar-section-end">
                  <mu-button v-on:click.prevent="remove(index)" color="error" small>წაშლა</mu-button>
                </div>
              </md-toolbar>
            </md-card>
            <md-divider></md-divider>
          </div>
        </transition-group>
      </draggable>
    </md-content>
  </mu-row>
</template>
<script>
    /** Components */
    import draggable from "vuedraggable";

    export default {
        name: "days",
        props: ['days'],
        components:{ draggable },
        data: function(){
            return {
                data:[],
            }
        },
        computed:{
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
            remove: function(data, index){
                return false;
            },
            render: function(val){
              this.data = [];
              let i;
              for (i = 0; i < val; i++) {
                this.data.push({index: i, day: i + 1, description: []});
              }
            }
        },
        created: function(){
          this.render(this.days);
        },
        watch:{
          days: function(val){
            this.render(this.days);
          },
          data: function(val){
            this.$emit("getDays", val);
          }
        }
    }
</script>