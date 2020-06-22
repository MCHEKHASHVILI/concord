<style scoped>
.md-scrollbar {
  height: 400px;
}
.md-content {
  max-height: 210px;
  /*  min-width: 300px; */
  width: 100%;
  overflow: scroll;
}
.d-flex {
  margin: 5px;
}
</style>
<template>
  <mu-flex class="flex-wrapper" align-items="start" style="width:100%">
    <mu-flex class="flex-demo" justify-content="center" fill>
      <mu-paper class="demo-paper" :z-depth="1" style="width:100%; padding:5px;">
        <mu-container class="demo-container" style="width:100%">
          <mu-form
            :model="form"
            class="mu-demo-form"
            label-position="top"
            label-width="100"
            style="max-width:100%"
          >
            <mu-row gutter>
              <mu-col span="10">
                <mu-form-item prop="select" label="ქვეყანა">
                  <mu-select
                    v-model="template.country"
                    name="template.country"
                    id="template.country"
                    placeholder="ამოირჩიეთ ქვეყანა"
                    v-bind:prop="{'error-text':CountryError}"
                  >
                    <mu-option
                      v-for="(option,index) in countries"
                      :key="option + index"
                      :label="option.name"
                      :value="option.code"
                    >{{option.name}}</mu-option>
                  </mu-select>
                </mu-form-item>
              </mu-col>
              <mu-col span="2" style="width:100%;">
                <mu-form-item class="float-right">
                  <mu-button color="success" small v-on:click.prevent="add">დამატება</mu-button>
                </mu-form-item>
              </mu-col>
            </mu-row>
          </mu-form>
          <hr class="mu-divider" />
          <mu-row gutter>
            <md-content class="md-scrollbar">
              <draggable
                v-if="data.length > 0"
                v-model="data"
                v-bind="dragOptions"
                handle=".handle"
              >
                <transition-group>
                  <div v-bind:key="index + '-' + item.id" v-for="(item,index) in data">
                    <md-card>
                      <md-toolbar class="md-secondary">
                        <mu-button fab small color="secondary" class="handle">
                          <md-icon>menu</md-icon>
                        </mu-button>
                        <span
                          class="md-title"
                        >{{countries.filter(obj => obj.code == item.country)[0]['name']}}</span>
                        <div class="md-toolbar-section-end">
                          <mu-button color="primary" small @click="showDetails(index)">დეტალები</mu-button>
                          <mu-button color="secondary" fab small>{{item.days}}</mu-button>
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
          <hr class="mu-divider" />
          Days
          <hr class="mu-divider" />
          <day-list :days="data[showing.index].days" @getDays="getDays"></day-list>
        </mu-container>
      </mu-paper>
    </mu-flex>
    <mu-flex class="flex-demo" justify-content="center" fill>
      <mu-paper class="demo-paper" :z-depth="1" style="width:100%; padding:5px;">
        <mu-flex style="margin-left:20px;" class="flex-demo" justify-content="center" fill>
          <mu-flex justify-content="start" fill>
            <h3>{{showing.index + 1}} ეტაპი - {{countries.filter(obj => obj.code == data[showing.index].country)[0]['name']}}</h3>
          </mu-flex>
          <mu-flex justify-content="end" fill>
            <mu-text-field
              v-model="data[showing.index].days"
              type="number"
              label="დღეები"
              min="1"
              placeholder="დღეები"
            ></mu-text-field>
          </mu-flex>
        </mu-flex>
        <hr class="mu-divider" />
        <mu-row gutter style="margin-left:20px;">
          <mu-flex
            align-items="stretch"
            v-bind:key="event.key"
            v-for="event in events"
            style="padding-bottom: 8px;"
          >
            <mu-form :model="form">
              <h5>{{event.name}}</h5>
              <mu-form-item style="width:100%">
                <div class="select-control-group">
                  <mu-flex
                    class="select-control-row"
                    :key="option.id"
                    v-for="option in eventOptions"
                  >
                    <mu-radio
                      :checked="option.propName == data[showing.index][event.prop].type"
                      :value="option.propName"
                      v-model="data[showing.index][event.prop].type"
                      style
                      :label="option.name"
                    ></mu-radio>
                  </mu-flex>
                </div>
              </mu-form-item>
              <mu-form-item prop="select" label="ობიექტი">
                <mu-select
                  v-model="data[showing.index][event.prop].object"
                  v-bind:id="event.prop + data[showing.index][event.prop].type + '-object'"
                  v-bind:name="event.prop + data[showing.index][event.prop].type + '-object'"
                  placeholder="ამოირჩიეთ ობიექტი"
                >
                  <mu-option
                    v-bind:key="obj.id"
                    v-for="obj in objects.filter(obj => obj.type == data[showing.index][event.prop].type && obj.country == data[showing.index].country)"
                    v-bind:value="obj"
                    :label="obj.name"
                  >{{obj.name}}</mu-option>
                </mu-select>
              </mu-form-item>
              <mu-form-item label="დრო">
                <mu-date-input
                  type="time"
                  v-model="data[showing.index][event.prop].time"
                  v-bind:id="showing.index + event.prop"
                  v-bind:name="showing.index + event.prop"
                  actions
                  full-width
                ></mu-date-input>
              </mu-form-item>
              <mu-form-item label="კომენტარი">
                <mu-text-field 
                  type="text"
                  v-model="data[showing.index][event.prop].comment"
                  v-bind:id="showing.index + event.prop + '-comment'"
                  v-bind:name="showing.index + event.prop + '-comment'"
                  full-width
                ></mu-text-field >
              </mu-form-item>
            </mu-form>
          </mu-flex>
          <mu-flex align-items="stretch" style="padding-bottom: 8px;">
            <mu-form :model="form" label-position="top">
              <mu-form-item prop="checkbox" label="კვება">
                <mu-checkbox
                  v-model="data[showing.index].requested.beverage"
                  value="B"
                  label="Breakfast"
                ></mu-checkbox>
                <mu-checkbox
                  v-model="data[showing.index].requested.beverage"
                  value="L"
                  label="Lunch"
                ></mu-checkbox>
                <mu-checkbox
                  v-model="data[showing.index].requested.beverage"
                  value="D"
                  label="Dinner"
                ></mu-checkbox>
              </mu-form-item>
              <mu-form-item class="select-control-row">
                <mu-switch
                  v-model="data[showing.index].requested.leader.optional"
                  label="ტურ ლიდერი"
                ></mu-switch>
              </mu-form-item>
              <mu-form-item
                class="select-control-row"
                v-if="data[showing.index].requested.leader.optional"
              >
                <mu-flex inline-flex>
                  <mu-form-item prop="input" label="From Pax">
                    <mu-text-field v-model="leaders.from" type="number" min="1"></mu-text-field>
                  </mu-form-item>
                  <mu-form-item prop="input" label="FOC QTY">
                    <mu-text-field v-model="leaders.qty" type="number" min="1"></mu-text-field>
                  </mu-form-item>
                  <mu-form-item prop="select" label="ოთახი">
                    <mu-select v-model="leaders.room" filterable full-width>
                      <mu-option
                        v-for="(option,index) in rooms"
                        :key="option + index" 
                        :label="option.name"
                        :value="option.code"
                      ></mu-option>
                    </mu-select>
                  </mu-form-item>
                  <mu-button color="teal" fab small v-on:click.prevent="addLeader">
                    <mu-icon value="add"></mu-icon>
                  </mu-button>
                </mu-flex>
              </mu-form-item>
              <mu-form-item
                class="select-control-row"
                v-if="data[showing.index].requested.leader.optional"
              >
              <mu-list>
                <mu-list-item button :ripple="false" v-bind:key="leader + index"
                    v-for="(leader,index) in data[showing.index].requested.leader.data">
                  <mu-list-item-action>
                    <mu-icon value="inbox"></mu-icon>
                  </mu-list-item-action>
                  <mu-list-item-title>
                    {{leader.from}}-დან - {{leader.qty}} ლიდერი {{filteredRoom(leader.room)}} ოთახში
                  </mu-list-item-title>
                  <mu-list-item-action v-on:click="removeFoc(data[showing.index].requested.leader.data, index)">
                    <mu-icon value="delete" color="error"></mu-icon>
                  </mu-list-item-action>
                </mu-list-item>
              </mu-list>
              </mu-form-item>
            </mu-form>
          </mu-flex>
        </mu-row>
      </mu-paper>
    </mu-flex>
    <mu-row>
      <!-- <pre>{{data[showing.index]}}</pre> -->
      <!-- {{headingInfo}} -->
      <!-- {{heading}} -->
    </mu-row>
    <mu-snackbar :color="color.color" :open.sync="color.open">
      <mu-icon left :value="icon"></mu-icon>
      {{color.message}}
      <mu-button flat slot="action" color="#fff" @click="color.open = false">Close</mu-button>
    </mu-snackbar>
  </mu-flex>
</template>
<script>
/** Css */
//import M from "materialize-css";
/** JS */
/** Data */
import countries from "../../components/data/countries";
import eventOptions from "../../components/data/startOptions";
import objects from "../../components/data/objects";
import rooms from "../../components/data/rooms";
import days from "../platform/days";
/** Components */
import draggable from "vuedraggable";
/** Export Component */
export default {
  components: { draggable, 'day-list': days },
  props: ["combined", "headingInfo"],
  data: function() {
    return {
      events: [
        { name: "დაწყება", prop: "start" },
        { name: "დასრულება", prop: "end" }
      ],
      colors: ["success", "info", "error", "warning"],
      color: {
        color: "success",
        message: "Success",
        open: false,
        timeout: 3000
      },
      requested: false,
      CountryError: false, // Error in Country Select Field
      eventOptions: eventOptions,
      rooms: rooms,
      showing: { index: 0, state: true },
      objects: objects,
      startType: false,
      data: [],
      template: {},
      leaders: {},
      countries: countries,
      form: {
        input: "",
        select: "",
        date: "",
        radio: "",
        checkbox: [],
        switch: false,
        slider: 30,
        textarea: ""
      }
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
    },
    icon: function() {
      return {
        success: "check_circle",
        info: "info",
        warning: "priority_high",
        error: "warning"
      }[this.color.color];
    }
  },
  created: function() {
    this.random();
    this.$emit("getCountryList", this.data);
    this.$emit("showing",this.showing);

    this.heading = this.$options.props.headingInfo; // This info is from headingInfo
  },
  watch: {
    data: function(val){
      this.$emit("getCountryList", val);
    },
    showing: function(val){
      this.$emit("showing",val)
    }
  },
  methods: {
    filteredRoom: function(code){
      let roomName = rooms.filter(room => room.code == code);
      return roomName[0].name; 
    },
    setLeaders: function() {
      this.leaders = JSON.parse(
        JSON.stringify({
          from: 1,
          qty: 1,
          room: 'sngl'
        })
      );
    },
    getDays: function(val){
      this.data[this.showing.index].description = val;
    },
    openColorSnackbar: function() {
      if (this.color.timer) clearTimeout(this.color.timer);
      this.color.open = true;
      this.color.timer = setTimeout(() => {
        this.color.open = false;
      }, this.color.timeout);
    },
    setDefaults: function() {
      this.template = JSON.parse(
        JSON.stringify({
          country: "",
          days: 1,
          showing: false,
          requested: {
            beverage: [],
            leader: {
              optional: false,
              data: []
            }
          },
          start: {
            type: false,
            object: false,
            status: false,
            time: null,
            comment:'',
          },
          end: {
            type: false,
            object: false,
            status: false,
            time: null,
            comment:'',
          }
        })
      );
    },
    showDetails: function(val) {
      this.showing.index = val;
      this.showing.state = true;
    },
    addLeader: function() {
      this.data[this.showing.index].requested.leader.data.push(
        JSON.parse(JSON.stringify(this.leaders))
      );
      this.setLeaders();
    },
    add: function() {
      if (!this.template.days) {
        this.template.days = 0;
      }
      if (this.template.country) {
        this.data.push(JSON.parse(JSON.stringify(this.template)));
        this.setDefaults();
      } else {
        this.color.color = "error";
        this.color.message = "გთხოვთ ამოირჩიოთ ქვეყანა";
        this.openColorSnackbar();
      }
    },
    remove: function(index) {
      this.$delete(this.data, index);
    },
    removeFoc: function(data, index) {
      this.$delete(data, index);
    },
    random: function() {
      this.setDefaults();
      this.template.country = "ge";
      this.data.push(JSON.parse(JSON.stringify(this.template)));
      this.showing.index = 0;
      this.showing.state = true;
      this.setDefaults();
      this.setLeaders();
    }
  }
};
</script>
