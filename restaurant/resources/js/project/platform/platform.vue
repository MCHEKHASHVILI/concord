<style scopped>
.mu-demo-form {
  width: 100%;
  max-width: 460px;
}
.save{
    position:fixed;
	bottom:40px;
	right:40px;
}

</style>
<template>
<div>
    <mu-appbar mu-appbar style="width: 100%; position: sticky; top: 0; padding-left:256px;" color="primary">
        Create Project
    </mu-appbar>
    <div style="width: 100%; padding-left:256px;">
        <div style="padding: 20px;">
            <heading-info @getHeadingInfo="getHeadingInfo"></heading-info>
            <country-listing :combined="true" :headingInfo="projectData.headingInfo.requested" @getCountryList="getCountryList" @showing="showing"></country-listing>
            <details-content :days="renderedDays"></details-content>
            <mu-button v-on:click.prevent="save" color="blue" size="lg" class="save">Save</mu-button>
        </div>
    </div>
</div>
</template>
<script>
    import countryListing from '../platform/countryListing';
    import details from '../platform/details';
    import headingInfo from '../platform/headingInfo';
    import projects from '../../components/data/projects';
    export default {
        name: 'platform',
        components: {
            'country-listing': countryListing,
            'heading-info': headingInfo,
            'details-content': details
        },
        data: function(){
            return{
                labelPosition: 'top',
                projectData:{
                    showing: false,
                    headingInfo: false,
                    countryListing: false
                },
                form: {
                    input: '',
                    select: '',
                    date: '',
                    radio: '',
                    checkbox: [],
                    switch: false,
                    slider: 30,
                    textarea: ''
                }
            }
        },
        computed:{
            renderedDays: function(){
                let result = 1;
                if(this.projectData.countryListing[this.projectData.showing.index]){
                    result = this.projectData.countryListing[this.projectData.showing.index].days;
                }
                return result;
            }
        },
        methods:{
            save: function(){
                console.log(this.projectData);
                // Push Data Into Projects JS. (Save Project)
                projects.push(this.projectData);
                console.log(projects);
            },
            getCountryList: function(e){
                this.projectData.countryListing = e;
            },
            getHeadingInfo: function(e){
                this.projectData.headingInfo = e;
            },
            showing: function(e){
                this.projectData.showing = e;
            }
        }
    }
</script>