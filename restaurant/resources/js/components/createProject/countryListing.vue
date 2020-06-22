<style scoped>
.md-toolbar {
  min-width: 1.75rem;
}
.md-gutter {
  height: 400px;
}
.md-content {
  max-height: 210px;
  overflow: auto;
}
</style>
<template>
  <div class="md-layout md-gutter">
    <div class="md-layout-item md-size-40">
      <md-card>
        <md-card-header>
          <md-field>
            <md-select
              v-model="template.country"
              name="country"
              id="template.country"
              placeholder="ქვეყანა"
            >
              <md-option
                v-bind:key="item.id"
                v-for="item in countries"
                v-bind:value="item.name"
                :disabled="!combined"
              >{{item.name}}</md-option>
            </md-select>
          </md-field>

          <span class="md-title">
            <md-button
              class="md-dense md-raised md-primary"
              v-on:click.prevent="add"
              :disabled="!combined"
            >დამატება</md-button>
          </span>
        </md-card-header>
      </md-card>
      <md-content class="md-scrollbar">
        <draggable v-if="data.length > 0" v-model="data" v-bind="dragOptions" handle=".handle">
          <transition-group>
            <div v-bind:key="index + '-' + item.id" v-for="(item,index) in data">
              <md-card>
                <md-progress-bar class="md-accent" md-mode="determinate" :md-value="2"></md-progress-bar>
                <md-toolbar class="md-secondary">
                  <md-button class="md-icon-button md-raised md-primary handle">
                    <md-icon>menu</md-icon>
                  </md-button>
                  <span class="md-title">{{item.country}}</span>

                  <div class="md-toolbar-section-end">
                    <md-button class="md-raised md-primary" @click="showDetails(index)">დეტალები</md-button>
                    <md-button
                      class="md-icon-button md-raised md-primary float-left md-mini"
                    >{{item.days}}</md-button>
                    <md-button
                      v-on:click.prevent="remove(index)"
                      class="md-raised md-accent float-right md-mini"
                    >წაშლა</md-button>
                  </div>
                </md-toolbar>
              </md-card>
              <md-divider></md-divider>
            </div>
          </transition-group>
        </draggable>
      </md-content>
    </div>
    <div class="md-layout-item">
      <md-card>
        <md-card-content v-if="showing.state" class="md-layout">
          <div v-bind:key="event.key" v-for="event in events" class="md-layout-item">
            <h5>{{event.name}}</h5>

              <md-radio
                v-model="data[showing.index][event.prop].type"
                :value="option.propName"
                :checked="option.propName == data[showing.index][event.prop].type"
                :key="option.id"
                :class="{'md-primary':data[showing.index][event.prop].type}"
                class="body-1"
                v-for="option in eventOptions"
              >{{option.name}}</md-radio>

            <md-field>
              <md-select
                v-model="data[showing.index][event.prop].object"
                v-bind:id="data[showing.index][event.prop].type + '-object'"
                placeholder="ობიექტი"
              >
                <md-option
                  v-bind:key="obj.id"
                  v-for="obj in objects.filter(obj => obj.type == data[showing.index][event.prop].type)"
                  v-bind:value="obj.name"
                >{{obj.name}}</md-option>
              </md-select>
            </md-field>
            <input
              class="timepicker"
              type="text"
              v-model="data[showing.index][event.prop].time"
              aria-describedby="{'aproove' + event.prop}"
            />
          </div>
        </md-card-content>
      </md-card>
    </div>
  </div>
</template>
<script>
/** Css */
//import M from "materialize-css";
/** JS */
/** Data */
import countries from "../data/countries";
import eventOptions from "../data/startOptions";
import objects from "../data/objects";
/** Components */
import draggable from "vuedraggable";
/** Export Component */
export default {
  components: { draggable },
  props: ["combined"],
  data: function() {
    return {
      events: [
        { name: "დაწყება", prop: "start" },
        { name: "დასრულება", prop: "end" }
      ],
      eventOptions: eventOptions,
      showing:{index:null,state:false},
      objects: objects,
      startType: false,
      data: [],
      template: {},
      countries: countries
    };
  },
  computed: {
    dragOptions: function() {
      return {
        animation: 500,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },
  created: function() {
    this.random();
  },
  watch: {
    data: function(val) {
      this.$emit("getCountryList", val);
    }
  },
  methods: {
    setDefaults: function() {
      this.template = JSON.parse(
        JSON.stringify({
          country: "",
          days: 0,
          showing: false,
          start: {
            known: false,
            type: false,
            object: false,
            status: false,
            time: "00:00:00.000"
          },
          end: {
            known: false,
            type: false,
            object: false,
            status: false,
            time: "00:00:00.000"
          }
        })
      );
    },
    showDetails: function(val) {
      this.showing.index = val;
      this.showing.state = true;
    },
    add: function() {
      if (!this.template.days) {
        this.template.days = 0;
      }
      this.data.push(JSON.parse(JSON.stringify(this.template)));
      this.setDefaults();
    },
    remove: function(index) {
      this.$delete(this.data, index);
    },
    random: function() {
      this.setDefaults();
      this.template.country = "Georgia";
      this.data.push(JSON.parse(JSON.stringify(this.template)));
      this.showing.index = 0;
      this.showing.state = true;
      this.setDefaults();
    }
  }
};
</script>
