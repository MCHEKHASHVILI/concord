<style scopped>
.d-flex {
  margin: 5px;
}
</style>
<template>
  <div>
    <mu-flex class="flex-wrapper" align-items="start" style="width:100%">
      <mu-flex class justify-content="start" fill>
        <mu-row gutter style="width:100%">
          <mu-col span="6">
            <div class="grid-cell">
              <mu-paper class="demo-paper" :z-depth="1" style="width:100%; padding:5px;">
                <mu-form :model="data" class label-position="top" label-width="100">
                  <mu-form-item prop="input" label="დასახელება">
                    <mu-text-field v-model="data.name"></mu-text-field>
                  </mu-form-item>
                  <mu-row gutter style="width: 100" inline-flex>
                    <mu-col span="4">
                      <mu-form-item prop="input" label="მინიმალური">
                        <mu-text-field v-model="data.pax.min" type="number" min="1"></mu-text-field>
                      </mu-form-item>
                    </mu-col>
                    <mu-col span="4">
                      <mu-form-item prop="input" label="მაქსიმალური">
                        <mu-text-field v-model="data.pax.max" type="number" min="1"></mu-text-field>
                      </mu-form-item>
                    </mu-col>
                    <mu-col span="4">
                      <mu-form-item prop="input" label="ბიჯი">
                        <mu-text-field v-model="data.pax.step" type="number" min="1"></mu-text-field>
                      </mu-form-item>
                    </mu-col>
                  </mu-row>
                </mu-form>
              </mu-paper>
            </div>
          </mu-col>
          <mu-col span="6">
            <div class="grid-cell">
              <mu-paper class="demo-paper" :z-depth="1" style="width:100%; padding:5px;">
                <mu-form :model="data" class label-position="top" label-width="100">
                  <mu-form-item prop="select" label="დამკვეთები">
                    <mu-select v-model="data.customers" filterable multiple full-width>
                      <mu-option
                        v-for="(option,index) in customers"
                        :key="option + index"
                        :label="option.name"
                        :value="option.id"
                      ></mu-option>
                    </mu-select>
                  </mu-form-item>
                  <!-- ძირითადი ენა და ენები -->
                  <mu-row gutter style="width: 100" inline-flex>
                    <mu-col span="4">
                      <mu-form-item prop="select" label="ძირითადი ენა">
                        <mu-select v-model="data.language" filterable full-width>
                          <mu-option
                            v-for="(option,index) in languages"
                            :key="option + index"
                            :label="option.name"
                            :value="option.code"
                          ></mu-option>
                        </mu-select>
                      </mu-form-item>
                    </mu-col>
                    <mu-col span="8">
                      <mu-form-item prop="select" label="დამატებითი_ენები">
                        <mu-select v-model="data.languages" filterable multiple full-width>
                          <mu-option
                            v-for="(option,index) in languages"
                            :key="option + index"
                            :label="option.name"
                            :value="option.code"
                          ></mu-option>
                        </mu-select>
                      </mu-form-item>
                    </mu-col>
                  </mu-row>
                </mu-form>
              </mu-paper>
            </div>
          </mu-col>
        </mu-row>
      </mu-flex>
    </mu-flex>
    <mu-flex class="flex-wrapper" align-items="start" style="width:100%">
      <mu-flex class justify-content="start" fill>
        <mu-row gutter style="width:100%">
          <mu-col span="12">
            <div class="grid-cell">
              <mu-paper class="demo-paper" :z-depth="1" style="width:100%; padding:5px;">
                <mu-form :model="data" class label-position="top" label-width="100">
                  გლობალური მოთხოვნები
                  <mu-flex align-items="stretch" style="padding-bottom: 8px;">
                    <mu-form :model="data" label-position="top">
                      <mu-form-item prop="checkbox" label="კვება">
                        <mu-checkbox
                          v-model="data.requested.beverage.data"
                          value="B"
                          label="Breakfast"
                        ></mu-checkbox>
                        <mu-checkbox v-model="data.requested.beverage.data" value="L" label="Lunch"></mu-checkbox>
                        <mu-checkbox
                          v-model="data.requested.beverage.data"
                          value="D"
                          label="Dinner"
                        ></mu-checkbox>
                      </mu-form-item>
                      <mu-form-item class="select-control-row">
                        <mu-switch v-model="data.requested.tourLeader.status" label="ტურ ლიდერი"></mu-switch>
                      </mu-form-item>
                      <mu-form-item
                        class="select-control-row"
                        v-if="data.requested.tourLeader.status"
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
                        class="select-control-row col-6"
                        v-if="data.requested.tourLeader.status"
                      >
                        <mu-list>
                          <mu-list-item button :ripple="false" v-bind:key="leader + index"
                              v-for="(leader,index) in data.requested.tourLeader.data">
                            <mu-list-item-action>
                              <mu-icon value="inbox"></mu-icon>
                            </mu-list-item-action>
                            <mu-list-item-title>{{leader.from}}-დან - {{leader.qty}} ლიდერი {{filteredRoom(leader.room)}} ოთახში</mu-list-item-title>
                            <mu-list-item-action v-on:click="removeLeader(data.requested.tourLeader.data, index)">
                              <mu-icon value="delete" color="error"></mu-icon>
                            </mu-list-item-action>
                          </mu-list-item>
                        </mu-list>
                      </mu-form-item>
                    </mu-form>
                  </mu-flex>
                </mu-form>
              </mu-paper>
            </div>
          </mu-col>
        </mu-row>
      </mu-flex>
    </mu-flex>
  </div>
</template>
<script>
import customers from "../../components/data/customers";
import languages from "../../components/data/languages";
import rooms from "../../components/data/rooms";
export default {
  data: function() {
    return {
      labelPosition: "top",
      customers: customers,
      languages: languages,
      rooms: rooms,
      leaders: {},
      data: {
        name: "",
        customers: [],
        language: "",
        languages: [],
        requested: {
          beverage: {
            status: false,
            global: false,
            data: []
          },
          tourLeader: {
            status: false,
            global: false,
            data: []
          }
        },
        pax: {
          min: 1,
          max: 1,
          step: 1
        },
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
  watch: {
    data: function(val) {
      this.$emit("getHeadingInfo", val);
    }
  },
  created: function() {
    this.setLeaders();
    this.$emit("getHeadingInfo", this.data);
  },
  methods: {
    filteredRoom: function(code){
      let roomName = rooms.filter(room => room.code == code);
      return roomName[0].name; 
    },
    addLeader: function() {
      this.data.requested.tourLeader.data.push(JSON.parse(JSON.stringify(this.leaders)));
      this.setLeaders();
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
    removeLeader: function(data, index) {
      this.$delete(data, index);
    }
  }
};
</script>