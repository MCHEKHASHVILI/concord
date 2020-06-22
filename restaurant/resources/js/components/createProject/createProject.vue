<template>
<div class="">
    <!-- გვერდის ქუდი, ფილტრები -->
    <div class="md-layout md-gutter">
      <div class="md-layout-item">
        <md-switch 
          v-model="header.combined"
          v-bind:disabled="header.combinedByList" 
          class="md-primary"
          id="combined">
          <span v-if="header.combined">კომბინირებული</span>
          <span v-else>არაკომბინირებული</span>
        </md-switch>
      </div>
    </div>
    <md-divider></md-divider>


    <country-listing :combined="header.combined" @getCountryList="getCountryList"></country-listing>

  <div class="divider"></div>

  <!-- გენერალური ინფორმაციის კონტეინერი -->
  <div class="row">
    <div class="col-12">
      <form v-on:submit.prevent="saveProject">
        <div class="form-row">
          <div class="form-group col-12">
            <label for="header.name">პროექტის დასახელება</label>
            <input v-model="header.name" type="text" id="header.name" class="form-control form-control-sm">
          </div>
        </div>
        <div class="form-row">
          <div class="form-group col-12">
            <div class="input-group">
              <div class="input-group-prepend">
                <span style="width: 250px;" class="input-group-text">დამკვეთები</span>
              </div>
              <vue-select v-model="header.customers" :data="customersPicker" :v="1.0" multiple placeholder="ქვეყანა"></vue-select>
            </div>
          </div>
        </div>

        <div class="form-row">
          <div class="form-group col-12">
            <label>ტურისტების რაოდენობა და დათვლის ბიჯი</label>
            <div class="input-group">
              <div class="input-group-prepend">
                <span class="input-group-text" id="basic-addon1">min / max / step</span>
              </div>
              <input type="number" aria-label="First name" class="form-control" placeholder="min">
              <input type="number" aria-label="First name" class="form-control" placeholder="max">
              <input type="number" aria-label="First name" class="form-control" placeholder="step">
            </div>
          </div>
        </div>

        <div class="form-row">
          <div class="form-group col-4">
            <div class="input-group">
              <div class="input-group-prepend">
                <span style="width: 250px;" class="input-group-text">ძირითადი ენა</span>
              </div>
              <vue-select v-model="header.language" :data="languages" :v="1.0" placeholder=""></vue-select>

            </div>
          </div>

            <div class="form-group col-4">
            <div class="input-group">
              <div class="input-group-prepend">
                <span style="width: 250px;" class="input-group-text">ძირითადი ენა</span>
              </div>
              <vue-select v-model="header.languages" :data="languages" :v="1.0" placeholder="" multiple></vue-select>
            </div>
          </div>
        </div>
        <div class="divider"></div>
        <!-- form footer -->
        <button type="" class="btn btn-success btn-sm">დამატება</button>
      </form>
    </div>
  </div>
  <div class="divider"></div>
  <!-- დეტალიზაციის კონტეინერი -->
  <div class="row">
    <div class="page-header">
      <h4>დეტალები ...</h4>
    </div>
  
  </div>
</div>
</template>

<script>
    /** Javascript Section **/
    function pickerHelper(arr1,arr2){
    return arr1
        .filter(x => !arr2.includes(x))
        .concat(arr2.filter(x => !arr1.includes(x)));
    }

    /** Vue Section **/
    import Select from "../formInputs/Select.vue";
    import CountryListing from "../createProject/countryListing.vue"

    import objects from "../data/objects";
    
    import countries from "../data/countries";
    

    export default {
        components:{
          'vue-select': Select,
          'country-listing': CountryListing
        },
        data: function() {
            return {
                objects: objects,
                countryListing: CountryListing,
                countries: countries,
                languages: ['Georgian','German', 'English', 'Russian', "japan"],
                partners:  ['company1', 'company2', 'company3'],

                header: {
                    name: '',
                    startType: false,
                    startEndTime: false,
                    startPlace: false,
                    startTime: false,
                    endType: false,
                    endPlace: false,
                    endTime: false,
                    combined: true,
                    combinedByList:false,
                    days: {value: 0, declared: false},
                    daysOut: [],
                    language: false,
                    languages: [],
                    customers: []
                },
                temporaryCError: false,
                temporary: {
                    country: '',
                days: 0
                },
                details: [],
            }
        },
        created:function() {
          
        },
        methods: {
            saveProject: function(){
                console.log(this.header)
            },
            getCountryList:function(list){
                this.header.combinedByList = list.length > 1 ? true : false;
            },
            addDaysOut: function(){
                if(this.temporary.country.length === 0){
                // შეტყობინება
                this.temporaryCError = true;
            }else{
                this.temporaryCError = false;
                this.header.daysOut.push({country: this.temporary.country, days: this.temporary.days})
                    this.temporary.country = '';
                    this.temporary.days = 0;
            }
                
            },
            
            setDays: function(){
                if(this.header.days.declared == false){
                if(this.header.days.value <= 0){ // Checking Value of Days
                    return false
                }else{
                    this.header.days.declared = !this.header.days.declared
                }
            }else{
                this.header.days.declared = !this.header.days.declared
            }
                
            }
            
        },
        computed: {
            customersPicker: function(){
            return pickerHelper(this.partners,this.header.customers)
            },
            extraLanguagesPicker: function(){
            return pickerHelper(this.languages,this.header.languages)
            },
            filteredObjectsStart: function(){
                return this.objects.filter(item => item.type == this.header.startType)
            },
            filteredObjectsEnd: function(){
                return this.objects.filter(item => item.type == this.header.endType)
            }
        }
    }
</script>

<style scoped>

</style>