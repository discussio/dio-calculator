<template>
  <div id="app">
    <!--<div id="bootstrap3">
    <div class="container-fluid">
      <div class="row">
      <div class="col-md-9">
      <div class="row"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Project Type</h3></div><div class="panel-body"><projectrow></projectrow></div></div></div>
    <div class="row"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Consumer Recruiting Quotas</h3></div><div class="panel-body"><div style="text-align:center;"><label>Number of Consumer Segments:</label> <input style="width:50px;" v-model="segments" type="number" min="1" max="5"/></div><recruitingrow v-for="n in segments" v-bind:segments="n"></recruitingrow></div></div></div>
    <div class="row"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Professional Services</h3></div><div class="panel-body"><servicesrow></servicesrow></div></div></div>

      </div>
        <div class="col-md-3">
          <div class="row alert alert-success" style="text-align:center; font-size:24px;"><label>Total Price (estimated): ${{ totalPrice.toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,').replace('.00', '') }}</label></div>
          <div class="row">
            <div class="col-md-4"><label>Required No. of Participants:</label> {{ minParticipants }} </div>
            <div class="col-md-4" style="text-align:center;" v-bind:style="countSync"><label>Current No. of Participants:</label> {{ participantCount }} </div>
            <div class="col-md-4" style="text-align:center;"><label>Number of Consumer Segments: {{ segments }}</label></div>
          </div>
          <div class="row" style="text-align:center;"><alertrow v-bind:alertObj="alertObj"></alertrow></div>
          </div>
      </div>
    </div>
    </div>-->
    <div id="bootstrap4">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-4"></div>
        <div class="col-lg-4">
      <div class="btn-group" role="group">
        <button type="button" v-on:click="priceSetting = 0" id="paygButton" v-bind:class="btnPaygObj">Pay-as-you-go</button>
        <button type="button" v-on:click="priceSetting = 1" id="subButton" v-bind:class="btnSubObj">Subscription</button>
      </div>
          </div>
        <div class="col-lg-4"></div>
        </div>
      <div class="row" style="margin-bottom:5em;">
      <div class="col-lg-8">
       <div class="col-lg-12">
      <div class="row"><div class="col-lg-12"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Project Type</h3></div><div class="panel-body"><projectrow v-bind:priceSetting="priceSetting" v-bind:sumTranslator="sumTranslator"></projectrow></div></div></div></div>
         <div class="row"><div class="col-lg-12"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Consumer Recruiting Quotas</h3><h6 style="text-align:center;">(Assuming one group per session - price includes incentives)</h6></div><div class="panel-body"><!--<div style="text-align:center;"><label>Number of Consumer Segments:</label> <input style="width:50px;" v-model="segments" type="number" min="1" max="5"/></div>--><recruitingrow v-for="n in segments" v-bind:segments="n" v-bind:priceSetting="priceSetting" v-bind:currentSessionQty="currentSessionQty"></recruitingrow></div></div></div></div>
    <!--<div class="row"><div class="col-lg-12"><div class="panel panel-default"><div class="panel-heading"><h3 class="panel-title">Professional Services</h3></div><div class="panel-body"><servicesrow></servicesrow></div></div></div></div>-->
      </div>
      </div>
        <div class="col-lg-4 hidden-md-down">
          <div class="row alert alert-success" style="text-align:center; font-size:24px; margin-top:1em;"><label>Total Price (estimated): ${{ totalPrice.toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,').replace('.00', '') }}</label></div>
          <div class="row">
            <div class="col-lg-12" style="text-align:center;"><label>Required No. of Participants:</label> {{ minParticipants }} </div>
            <div class="col-lg-12" style="text-align:center;" v-bind:style="countSync"><label>Current No. of Participants:</label> {{ participantCount }} </div>
            <div class="col-lg-12" style="text-align:center;"><label>Number of Consumer Groups: {{ segments }}</label></div>
          </div>
          <div class="row" style="text-align:center;"><alertrow v-bind:alertObj="alertObj"></alertrow></div>
          </div>
      </div>
      <div class="row hidden-lg-up">
        <div class="col-sm-12">
          <div class="row alert alert-success" style="text-align:center; font-size:24px;"><label>Total Price (estimated): ${{ totalPrice.toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,').replace('.00', '') }}</label></div>
          <div class="row">
            <div class="col"><label>Required No. of Participants:</label> {{ minParticipants }} </div>
            <div class="col" style="text-align:center;" v-bind:style="countSync"><label>Current No. of Participants:</label> {{ participantCount }} </div>
            <div class="col" style="text-align:center;"><label>Number of Consumer Groups: {{ segments }}</label></div>
          </div>
          <div class="row" style="text-align:center;"><div class="col"><alertrow v-bind:alertObj="alertObj"></alertrow></div></div>
        </div>
        </div>
    </div>
    </div>
    </div>
</template>

<script>
import recruitingrow from './components/RecruitingRow'
import projectrow from './components/ProjectRow'
import servicesrow from './components/ServicesRow'
import alertrow from './components/AlertRow'

import eventHub from './main.js'
export default {
  name: 'app',
  props: ['rowPrice'],
  components: {
    recruitingrow,
    projectrow,
    servicesrow,
    alertrow
  },

  data () {
    return {
      segments: 1,
      totalPrice: 0,
      techAdded: null,
      minParticipants: 1,
      participantCount: 1,
      participantsPerSession: 1,
      sumTranslator: 0,
      countSync: {},
      nodePrices: [],
      alertObj: {},
      priceSetting: 0,
      currentSessionQty: 1,
      successMsg: 'You have the required number of participants for your project. Great!',
      errorMsg: 'The current number of participants does not meet the required amount.',
      defaultMsg: 'Proceed to configure this report to calculate your estimated project price.'
    }
    },

    watch: {
    segments: function (newVal, oldVal) {
    if (oldVal > newVal) {

    this.nodePrices = this.nodePrices.filter(function(el) {
          return el.id !== oldVal;
        });

    }

    },

    currentSessionQty: function(newVal) {
        this.segments = newVal;
    },

    minParticipants: function() {
     this.countSync = (this.minParticipants != this.participantCount) ? ({ color:"red" }) : ({color: "green"});

     this.alertObj =
         {
         "class" : (this.minParticipants != this.participantCount) ? ("alert alert-danger") : ("alert alert-success"),
         "content" : (this.minParticipants != this.participantCount ? this.errorMsg : this.successMsg),
         "participantCount" : this.participantCount,
         "defaultClass" : "alert alert-info",
         "defaultContent" : this.defaultMsg
         }


    },
    nodePrices: function () {
    // console.log(this.nodePrices);
    if(this.nodePrices.length > 0) {

      var sumPrice = 0;
      var countParticipants = 0;
      var countTranslators = 0;
      var translatorCost = 0;
        for (var i in this.nodePrices) {
          sumPrice += this.nodePrices[i]["atts"][0]["price"];
          if (this.nodePrices[i]["atts"][0]["participantQty"]) {
            countParticipants += this.nodePrices[i]["atts"][0]["participantQty"];
          }
          if (this.nodePrices[i]["atts"][0]["translatorCost"] > 0) {
              // console.log(this.nodePrices[i]["atts"][0]["translatorCost"]);
            countTranslators += 1;
            translatorCost = +this.nodePrices[i]["atts"][0]["translatorCost"];
          }
        }

      switch(this.currentSessionQty == this.segments) {
        case true:
          this.sumTranslator = (countTranslators === 0 ? 0 : (translatorCost*countTranslators));
          break;
        default:
          this.sumTranslator = (countTranslators === 0 ? 0 : (translatorCost*(this.currentSessionQty))/countTranslators);
      }


      this.participantCount = countParticipants;
      this.countSync = (this.minParticipants != this.participantCount) ? ({ color:"red" }) : ({color: "green"});

      this.alertObj =
        {
        "class" : (this.minParticipants != this.participantCount) ? ("alert alert-danger") : ("alert alert-success"),
        "content" : (this.minParticipants != this.participantCount ? this.errorMsg : this.successMsg),
        "participantCount" : this.participantCount,
        "defaultClass" : "alert alert-info",
        "defaultContent" : this.defaultMsg
        }
      this.totalPrice = ( sumPrice == 0 ? this.totalPrice : sumPrice ) + +this.sumTranslator;

         // this.participantCount = countParticipants;

      }
    }
    },

    methods: {
    addPrice: function (newPrice) {
    // console.log(newPrice);


      if(this.nodePrices.length > 0) {
        for (var i in this.nodePrices) {
          this.nodePrices = this.nodePrices.filter(function(el) {
            return el.id !== newPrice.id;
          });
        }
      }
      this.nodePrices.push(newPrice);


    },
    adjSessionQty: function (sessionQty) {
      // this.totalPrice = this.totalPrice + (sessionQty * 299);
      this.currentSessionQty = sessionQty;
    },
    adjMinParticipants: function (total) {
      this.minParticipants = total;
    },
    adjParticipantsPerSession: function (val) {
        this.participantsPerSession = val;

    }
    },
  computed: {
      btnPaygObj: function() {
          return {
            'btn': true,
            'btn-secondary': (this.priceSetting !== 0),
            'btn-active': (this.priceSetting === 0)
          }
      },
    btnSubObj: function() {
      return {
        'btn': true,
        'btn-secondary': (this.priceSetting === 0),
        'btn-active': (this.priceSetting !== 0)
      }
    }
  },
  created() {
    console.log('created');
    this.totalPrice = 299;
    eventHub.$on('recruitingPrice', this.addPrice);
    eventHub.$on('projectRowPrice', this.addPrice);
    eventHub.$on('servicesRowPrice', this.addPrice);
    eventHub.$on('sessionQty', this.adjSessionQty);
    eventHub.$on('participantQty', this.addPrice);
    eventHub.$on('minParticipants', this.adjMinParticipants);
    eventHub.$on('currentSessionQty', this.adjSessionQty);
    eventHub.$on('translator', this.addPrice);
    eventHub.$on('participantsPerSession', this.adjParticipantsPerSession);
  },
  mounted() {
  }
}
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;

}
</style>
