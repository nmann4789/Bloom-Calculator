<template>
  <div>
    <div class="calculator">
      <form v-on:submit.prevent="calculateSavings">
        <div class="form-header">
          <h2>Expected Savings Calculator</h2>
          <p>Enter your company(s) current annual insurance carrier premium spend for each of the P&C lines of business provided below to calculate the potential savings with a captive solution. Note the projected savings is based on a combination of aggregated statutory financial statement data for insurance carriers and Spring's experience with these lines of business in captive insurance companies.</p>
        </div>
        <div>
          <label for="commercialAutoLiability">Commercial Auto Liability:</label>
          <input required type="number" v-model="commercialAutoLiability.formInput" @change="handleCommercialAutoLiability(commercialAutoLiability.formInput)" name="commercialAutoLiability" placeholder="Enter Cost ($)">
        </div>
        <div>
          <label for="otherLiability">Other Liability (Claims-Made):</label>
          <input required type="number" v-model="otherLiability.formInput" @change="handleOtherLiability(otherLiability.formInput)" name="otherLiability" placeholder="Enter Cost ($)">
        </div>
        <div>
          <label for="commercialProperty">Commercial Property:</label>
          <input required type="number" v-model="commercialProperty.formInput" @change="handleCommercialProperty(commercialProperty.formInput)" ame="commercialProperty" placeholder="Enter Cost ($)">
        </div>
        <div>
          <label for="workersComp">Workers Compensation:</label>
          <input requried type="number" v-model="workersComp.formInput" @change="handleWorkersComp(workersComp.formInput)" name="workersComp" placeholder="Enter Cost ($)">
        </div>
        <div>
          <label for="otherCoverages">Other Coverages:</label>
          <input required type="number" v-model="otherCoverages.formInput" @change="handleOtherCoverages(otherCoverages.formInput)" name="otherCoverages" placeholder="Enter Cost ($)">
        </div>
        <div>
          <label></label>
          <input type="submit" class="submit" value="Calculate"/>
        </div>
      </form>
    </div>
    <div class="results" v-if="showResults">
      <h3 class="center">Expected Savings with a Captive - Projected and Last 3 Years
        (as a % of Your Current Commercial Insurance Carrier Premiums)</h3>
      <div>
        <Chart :chart-data="this.chartData"/>
      </div>
      <div class="breakdown">
        <h3 class="total-savings">Projected savings for 2019 based on inputted carrier premiums: <span>${{this.projectedSavings2019}}</span></h3>
        <h4 class="">{{this.totalProfits.total2019}}% in projected annual premium savings for 2019 comes from three key sources:</h4>
        <ul>
          <li>{{this.totalExpenseDifferences.total2019}}% is from the captive's expected lower operating costs compared to commercial carriers</li>
          <li>{{this.totalIncomeOnInsTransaction.total2019}}% is from retained investment income on reserve balances used to pay future paid losses</li>
          <li>{{this.totalUWProfitLoss.total2019}}% in underwriting income (losses shown as a negative value) retained by the captive</li>
        </ul>
        <p class="notes">Note these projections are based on assumptions established by our internal actuarial team using a combination of aggregated statutory financial statement loss and expense data from S&P Global and Spring's captive experience. Potential savings may be more or less based differing assumptions such as the use of a front/reinsurance. This assumes the following captive retained risk percentages: Commercial Auto Liability: {{convertToPercentage(commercialAutoLiability.captiveRetainedRiskPercentage)}}; Other Liability (Claims-Made): {{convertToPercentage(otherLiability.captiveRetainedRiskPercentage)}}; Commercial Property: {{convertToPercentage(commercialProperty.captiveRetainedRiskPercentage)}}; Workers' Compensation: {{convertToPercentage(workersComp.captiveRetainedRiskPercentage)}}; Other Coverages: {{convertToPercentage(otherCoverages.captiveRetainedRiskPercentage)}}</p>
        <p>Share these results by copying the following link:<a href="/calculator?cal="commercialAutoLiability.formInput></a></p>
      </div>
    </div>
  </div>
</template>
<script>

import Chart from './Chart'

export default {
  name: 'Calculator',
  components:{
    Chart
  },
  data () {
    return {
      bPreviousResults:true,
      chartData:Object,
      showResults:false,
      commercialAutoLiability:{
        formInput:undefined,
        captiveRetainedRiskPercentage:0.75,
        netCaptivePremium:0,
        incomeOnInsTransaction:{
          '2016':0.056,
          '2017':0.059,
          '2018':0.056,
          '2019':0.057
        },
        uWProfitLoss:{
          '2016':-0.124,
          '2017':-0.139,
          '2018':-0.131,
          '2019':-0.1312
        },
        expenseDifferences:{
          '2016':0.174,
          '2017':0.175,
          '2018':0.179,
          '2019':0.176
        },
        totalProfits:{
          '2016':0,
          '2017':0,
          '2018':0,
          '2019':0
        }
      },
      otherLiability:{
        formInput:undefined,
        captiveRetainedRiskPercentage:1.00,
        netCaptivePremium:0,
        incomeOnInsTransaction:{
          '2016':0.212,
          '2017':0.12,
          '2018':0.102,
          '2019':0.145
        },
        uWProfitLoss:{
          '2016':-0.063,
          '2017':0.024,
          '2018':0.059,
          '2019':0.0067
        },
        expenseDifferences:{
          '2016':0.322,
          '2017':0.251,
          '2018':0.282,
          '2019':0.285
        },
        totalProfits:{
          '2016':0,
          '2017':0,
          '2018':0,
          '2019':0
        }
      },
      commercialProperty:{
        formInput:undefined,
        captiveRetainedRiskPercentage:0.75,
        netCaptivePremium:0,
        incomeOnInsTransaction:{
          '2016':0.021,
          '2017':0.031,
          '2018':0.025,
          '2019':0.025
        },
        uWProfitLoss:{
          '2016':0.019,
          '2017':-0.119,
          '2018':-0.067,
          '2019':-0.0558
        },
        expenseDifferences:{
          '2016':0.219,
          '2017':0.241,
          '2018':0.248,
          '2019':0.236
        },
        totalProfits:{
          '2016':0,
          '2017':0,
          '2018':0,
          '2019':0
        }
      },
      workersComp:{
        formInput:undefined,
        captiveRetainedRiskPercentage:0.90,
        netCaptivePremium:0,
        incomeOnInsTransaction:{
          '2016':0.131,
          '2017':0.136,
          '2018':0.118,
          '2019':0.128
        },
        uWProfitLoss:{
          '2016':0.042,
          '2017':0.079,
          '2018':0.123,
          '2019':0.081
        },
        expenseDifferences:{
          '2016':0.236,
          '2017':0.254,
          '2018':0.253,
          '2019':0.248
        },
        totalProfits:{
          '2016':0,
          '2017':0,
          '2018':0,
          '2019':0
        }
      },
      otherCoverages:{
        formInput:undefined,
        captiveRetainedRiskPercentage:1.00,
        netCaptivePremium:0,
        incomeOnInsTransaction:{
          '2016':0.052,
          '2017':0.052,
          '2018':0.045,
          '2019':0.050
        },
        uWProfitLoss:{
          '2016':-0.009,
          '2017':-0.055,
          '2018':0.003,
          '2019':-0.020
        },
        expenseDifferences:{
          '2016':0.276,
          '2017':0.285,
          '2018':0.267,
          '2019':0.276
        },
        totalProfits:{
          '2016':0,
          '2017':0,
          '2018':0,
          '2019':0
        }
      },
      totalInsuredsPremium:0,
      totalIncomeOnInsTransaction:{
        total2016:0,
        total2017:0,
        total2018:0,
        total2019:0,
      },
      totalUWProfitLoss:{
        total2016:0,
        total2017:0,
        total2018:0,
        total2019:0,
      },
      totalExpenseDifferences:{
        total2016:0,
        total2017:0,
        total2018:0,
        total2019:0,
      },
      totalProfits:{
        total2016:0,
        total2017:0,
        total2018:0,
        total2019:0,
      },
      projectedSavings2019:0,
      shareLink:''
    }
  },
  created () {

  },
  mounted () {
    this.checkForExistingResults()
    this.setTotalProfits()
    if(this.bPreviousResults){
      this.calculateSavings()
    }
  },
  methods: {
    getParameterByName: function (name) {
      name = name.replace(/[[]/, '\\[').replace(/[\]]/, '\\]')
      let regex = new RegExp('[\\?&]' + name + '=([^&#]*)')
      let results = regex.exec(window.location.search)
      if(results == null){
        console.log("setting share link to false")
        this.bPreviousResults = false
      }
      return (results === null) ? '' : decodeURIComponent(results[1].replace(/\+/g, ' '))
    },
    checkForExistingResults : function (){
      console.log("checking for existing results")
      this.commercialAutoLiability.formInput = parseInt(this.getParameterByName('cal'))
      this.otherLiability.formInput = parseInt(this.getParameterByName('ol'))
      this.commercialProperty.formInput = parseInt(this.getParameterByName('cp'))
      this.workersComp.formInput = parseInt(this.getParameterByName('wc'))
      this.otherCoverages.formInput = parseInt(this.getParameterByName('oc'))
      if(this.bPreviousResults){
        this.handleCommercialAutoLiability (this.commercialAutoLiability.formInput)
        this.handleOtherLiability(this.otherLiability.formInput)
        this.handleWorkersComp(this.workersComp.formInput)
        this.handleCommercialProperty(this.commercialProperty.formInput)
        this.handleOtherCoverages(this.otherCoverages.formInput)
      }else{
        console.log("new calculator")
      }
    },
    setShareLink: function () {
      this.shareLink =  "/calculator?cal="+this.commercialAutoLiability.formInput+"&ol="+this.otherLiability.formInput+"&cp="+this.commercialProperty.formInput+"&wc="+this.workersComp.formInput+"&oc="+this.otherCoverages.formInput
      console.log(this.shareLink)
    },
    calculateSavings: function (event){
      this.totalInsuredsPremium = this.commercialAutoLiability.formInput + this.otherLiability.formInput + this.workersComp.formInput + this.commercialProperty.formInput + this.otherCoverages.formInput
      console.log(" this.totalInsuredsPremium: "+this.totalInsuredsPremium)
      this.showResults = true
      this.calculateTotalIncomeOnInsTransaction()
      this.calculateTotalUWProfitLoss()
      this.calculateTotalExpenseDifferences()
      this.calculateTotalProfits()
      this.fillDataForChart()
      this.setShareLink()
    },
    convertToPercentage: function (decimal){
      return decimal*100+"%"
    },
    handleCommercialAutoLiability: function (input){
      this.commercialAutoLiability.formInput = parseInt(input)
      this.commercialAutoLiability.netCaptivePremium = parseInt(input) * this.commercialAutoLiability.captiveRetainedRiskPercentage
    },
    handleOtherLiability: function (input){
      this.otherLiability.formInput = parseInt(input)
      this.otherLiability.netCaptivePremium = parseInt(input) * this.otherLiability.captiveRetainedRiskPercentage
    },
    handleWorkersComp: function (input){
      this.workersComp.formInput = parseInt(input)
      this.workersComp.netCaptivePremium = parseInt(input) * this.workersComp.captiveRetainedRiskPercentage
    },
    handleCommercialProperty: function (input){
      this.commercialProperty.formInput = parseInt(input)
      this.commercialProperty.netCaptivePremium = parseInt(input) * this.commercialProperty.captiveRetainedRiskPercentage
    },
    handleOtherCoverages: function (input){
      this.otherCoverages.formInput = parseInt(input)
      this.otherCoverages.netCaptivePremium = parseInt(input) * this.otherCoverages.captiveRetainedRiskPercentage
    },
    setTotalProfits: function () {
      console.log("setting total profits")
      this.commercialAutoLiability.totalProfits['2016'] = this.commercialAutoLiability.incomeOnInsTransaction['2016'] + this.commercialAutoLiability.uWProfitLoss['2016'] + this.commercialAutoLiability.expenseDifferences['2016']
      this.otherLiability.totalProfits['2016'] = this.otherLiability.incomeOnInsTransaction['2016'] + this.otherLiability.uWProfitLoss['2016'] + this.otherLiability.expenseDifferences['2016']
      this.workersComp.totalProfits['2016'] = this.workersComp.incomeOnInsTransaction['2016'] + this.workersComp.uWProfitLoss['2016'] + this.workersComp.expenseDifferences['2016']
      this.commercialProperty.totalProfits['2016'] = this.commercialProperty.incomeOnInsTransaction['2016'] + this.commercialProperty.uWProfitLoss['2016'] + this.commercialProperty.expenseDifferences['2016']
      this.otherCoverages.totalProfits['2016'] = this.otherCoverages.incomeOnInsTransaction['2016'] + this.otherCoverages.uWProfitLoss['2016'] + this.otherCoverages.expenseDifferences['2016']

      this.commercialAutoLiability.totalProfits['2017'] = this.commercialAutoLiability.incomeOnInsTransaction['2017'] + this.commercialAutoLiability.uWProfitLoss['2017'] + this.commercialAutoLiability.expenseDifferences['2017']
      this.otherLiability.totalProfits['2017'] = this.otherLiability.incomeOnInsTransaction['2017'] + this.otherLiability.uWProfitLoss['2017'] + this.otherLiability.expenseDifferences['2017']
      this.workersComp.totalProfits['2017'] = this.workersComp.incomeOnInsTransaction['2017'] + this.workersComp.uWProfitLoss['2017'] + this.workersComp.expenseDifferences['2017']
      this.commercialProperty.totalProfits['2017'] = this.commercialProperty.incomeOnInsTransaction['2017'] + this.commercialProperty.uWProfitLoss['2017'] + this.commercialProperty.expenseDifferences['2017']
      this.otherCoverages.totalProfits['2017'] = this.otherCoverages.incomeOnInsTransaction['2017'] + this.otherCoverages.uWProfitLoss['2017'] + this.otherCoverages.expenseDifferences['2017']

      this.commercialAutoLiability.totalProfits['2018'] = this.commercialAutoLiability.incomeOnInsTransaction['2018'] + this.commercialAutoLiability.uWProfitLoss['2018'] + this.commercialAutoLiability.expenseDifferences['2018']
      this.otherLiability.totalProfits['2018'] = this.otherLiability.incomeOnInsTransaction['2018'] + this.otherLiability.uWProfitLoss['2018'] + this.otherLiability.expenseDifferences['2018']
      this.workersComp.totalProfits['2018'] = this.workersComp.incomeOnInsTransaction['2018'] + this.workersComp.uWProfitLoss['2018'] + this.workersComp.expenseDifferences['2018']
      this.commercialProperty.totalProfits['2018'] = this.commercialProperty.incomeOnInsTransaction['2018'] + this.commercialProperty.uWProfitLoss['2018'] + this.commercialProperty.expenseDifferences['2018']
      this.otherCoverages.totalProfits['2018'] = this.otherCoverages.incomeOnInsTransaction['2018'] + this.otherCoverages.uWProfitLoss['2018'] + this.otherCoverages.expenseDifferences['2018']

      this.commercialAutoLiability.totalProfits['2019'] = this.commercialAutoLiability.incomeOnInsTransaction['2019'] + this.commercialAutoLiability.uWProfitLoss['2019'] + this.commercialAutoLiability.expenseDifferences['2019']
      this.otherLiability.totalProfits['2019'] = this.otherLiability.incomeOnInsTransaction['2019'] + this.otherLiability.uWProfitLoss['2019'] + this.otherLiability.expenseDifferences['2019']
      this.workersComp.totalProfits['2019'] = this.workersComp.incomeOnInsTransaction['2019'] + this.workersComp.uWProfitLoss['2019'] + this.workersComp.expenseDifferences['2019']
      this.commercialProperty.totalProfits['2019'] = this.commercialProperty.incomeOnInsTransaction['2019'] + this.commercialProperty.uWProfitLoss['2019'] + this.commercialProperty.expenseDifferences['2019']
      this.otherCoverages.totalProfits['2019'] = this.otherCoverages.incomeOnInsTransaction['2019'] + this.otherCoverages.uWProfitLoss['2019'] + this.otherCoverages.expenseDifferences['2019']
    },
    calculateTotalIncomeOnInsTransaction: function () {
      let commercialAutoLiability_2016 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.incomeOnInsTransaction['2016']
      let otherLiability_2016 = this.otherLiability.netCaptivePremium * this.otherLiability.incomeOnInsTransaction['2016']
      let commercialProperty_2016 = this.commercialProperty.netCaptivePremium * this.commercialProperty.incomeOnInsTransaction['2016']
      let workersComp_2016 = this.workersComp.netCaptivePremium * this.workersComp.incomeOnInsTransaction['2016']
      let otherCoverages_2016 = this.otherCoverages.netCaptivePremium * this.otherCoverages.incomeOnInsTransaction['2016']

      let commercialAutoLiability_2017 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.incomeOnInsTransaction['2017']
      let otherLiability_2017 = this.otherLiability.netCaptivePremium * this.otherLiability.incomeOnInsTransaction['2017']
      let commercialProperty_2017 = this.commercialProperty.netCaptivePremium * this.commercialProperty.incomeOnInsTransaction['2017']
      let workersComp_2017 = this.workersComp.netCaptivePremium * this.workersComp.incomeOnInsTransaction['2017']
      let otherCoverages_2017 = this.otherCoverages.netCaptivePremium * this.otherCoverages.incomeOnInsTransaction['2017']

      let commercialAutoLiability_2018 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.incomeOnInsTransaction['2018']
      let otherLiability_2018 = this.otherLiability.netCaptivePremium * this.otherLiability.incomeOnInsTransaction['2018']
      let commercialProperty_2018 = this.commercialProperty.netCaptivePremium * this.commercialProperty.incomeOnInsTransaction['2018']
      let workersComp_2018 = this.workersComp.netCaptivePremium * this.workersComp.incomeOnInsTransaction['2018']
      let otherCoverages_2018 = this.otherCoverages.netCaptivePremium * this.otherCoverages.incomeOnInsTransaction['2018']

      let commercialAutoLiability_2019 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.incomeOnInsTransaction['2019']
      let otherLiability_2019 = this.otherLiability.netCaptivePremium * this.otherLiability.incomeOnInsTransaction['2019']
      let commercialProperty_2019 = this.commercialProperty.netCaptivePremium * this.commercialProperty.incomeOnInsTransaction['2019']
      let workersComp_2019 = this.workersComp.netCaptivePremium * this.workersComp.incomeOnInsTransaction['2019']
      let otherCoverages_2019 = this.otherCoverages.netCaptivePremium * this.otherCoverages.incomeOnInsTransaction['2019']

      this.totalIncomeOnInsTransaction.total2016 = ((commercialAutoLiability_2016+otherLiability_2016+commercialProperty_2016+workersComp_2016+otherCoverages_2016)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalIncomeOnInsTransaction.total2017 = ((commercialAutoLiability_2017+otherLiability_2017+commercialProperty_2017+workersComp_2017+otherCoverages_2017)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalIncomeOnInsTransaction.total2018 = ((commercialAutoLiability_2018+otherLiability_2018+commercialProperty_2018+workersComp_2018+otherCoverages_2018)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalIncomeOnInsTransaction.total2019 = ((commercialAutoLiability_2019+otherLiability_2019+commercialProperty_2019+workersComp_2019+otherCoverages_2019)/this.totalInsuredsPremium*100).toFixed(2)
    },
    calculateTotalUWProfitLoss: function () {
      let commercialAutoLiability_2016 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.uWProfitLoss['2016']
      let otherLiability_2016 = this.otherLiability.netCaptivePremium * this.otherLiability.uWProfitLoss['2016']
      let commercialProperty_2016 = this.commercialProperty.netCaptivePremium * this.commercialProperty.uWProfitLoss['2016']
      let workersComp_2016 = this.workersComp.netCaptivePremium * this.workersComp.uWProfitLoss['2016']
      let otherCoverages_2016 = this.otherCoverages.netCaptivePremium * this.otherCoverages.uWProfitLoss['2016']

      let commercialAutoLiability_2017 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.uWProfitLoss['2017']
      let otherLiability_2017 = this.otherLiability.netCaptivePremium * this.otherLiability.uWProfitLoss['2017']
      let commercialProperty_2017 = this.commercialProperty.netCaptivePremium * this.commercialProperty.uWProfitLoss['2017']
      let workersComp_2017 = this.workersComp.netCaptivePremium * this.workersComp.uWProfitLoss['2017']
      let otherCoverages_2017 = this.otherCoverages.netCaptivePremium * this.otherCoverages.uWProfitLoss['2017']

      let commercialAutoLiability_2018 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.uWProfitLoss['2018']
      let otherLiability_2018 = this.otherLiability.netCaptivePremium * this.otherLiability.uWProfitLoss['2018']
      let commercialProperty_2018 = this.commercialProperty.netCaptivePremium * this.commercialProperty.uWProfitLoss['2018']
      let workersComp_2018 = this.workersComp.netCaptivePremium * this.workersComp.uWProfitLoss['2018']
      let otherCoverages_2018 = this.otherCoverages.netCaptivePremium * this.otherCoverages.uWProfitLoss['2018']

      let commercialAutoLiability_2019 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.uWProfitLoss['2019']
      let otherLiability_2019 = this.otherLiability.netCaptivePremium * this.otherLiability.uWProfitLoss['2019']
      let commercialProperty_2019 = this.commercialProperty.netCaptivePremium * this.commercialProperty.uWProfitLoss['2019']
      let workersComp_2019 = this.workersComp.netCaptivePremium * this.workersComp.uWProfitLoss['2019']
      let otherCoverages_2019 = this.otherCoverages.netCaptivePremium * this.otherCoverages.uWProfitLoss['2019']

      this.totalUWProfitLoss.total2016 = ((commercialAutoLiability_2016+otherLiability_2016+commercialProperty_2016+workersComp_2016+otherCoverages_2016)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalUWProfitLoss.total2017 = ((commercialAutoLiability_2017+otherLiability_2017+commercialProperty_2017+workersComp_2017+otherCoverages_2017)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalUWProfitLoss.total2018 = ((commercialAutoLiability_2018+otherLiability_2018+commercialProperty_2018+workersComp_2018+otherCoverages_2018)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalUWProfitLoss.total2019 = ((commercialAutoLiability_2019+otherLiability_2019+commercialProperty_2019+workersComp_2019+otherCoverages_2019)/this.totalInsuredsPremium*100).toFixed(2)
    },
    calculateTotalExpenseDifferences: function () {
      let commercialAutoLiability_2016 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.expenseDifferences['2016']
      let otherLiability_2016 = this.otherLiability.netCaptivePremium * this.otherLiability.expenseDifferences['2016']
      let commercialProperty_2016 = this.commercialProperty.netCaptivePremium * this.commercialProperty.expenseDifferences['2016']
      let workersComp_2016 = this.workersComp.netCaptivePremium * this.workersComp.expenseDifferences['2016']
      let otherCoverages_2016 = this.otherCoverages.netCaptivePremium * this.otherCoverages.expenseDifferences['2016']

      let commercialAutoLiability_2017 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.expenseDifferences['2017']
      let otherLiability_2017 = this.otherLiability.netCaptivePremium * this.otherLiability.expenseDifferences['2017']
      let commercialProperty_2017 = this.commercialProperty.netCaptivePremium * this.commercialProperty.expenseDifferences['2017']
      let workersComp_2017 = this.workersComp.netCaptivePremium * this.workersComp.expenseDifferences['2017']
      let otherCoverages_2017 = this.otherCoverages.netCaptivePremium * this.otherCoverages.expenseDifferences['2017']

      let commercialAutoLiability_2018 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.expenseDifferences['2018']
      let otherLiability_2018 = this.otherLiability.netCaptivePremium * this.otherLiability.expenseDifferences['2018']
      let commercialProperty_2018 = this.commercialProperty.netCaptivePremium * this.commercialProperty.expenseDifferences['2018']
      let workersComp_2018 = this.workersComp.netCaptivePremium * this.workersComp.expenseDifferences['2018']
      let otherCoverages_2018 = this.otherCoverages.netCaptivePremium * this.otherCoverages.expenseDifferences['2018']

      let commercialAutoLiability_2019 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.expenseDifferences['2019']
      let otherLiability_2019 = this.otherLiability.netCaptivePremium * this.otherLiability.expenseDifferences['2019']
      let commercialProperty_2019 = this.commercialProperty.netCaptivePremium * this.commercialProperty.expenseDifferences['2019']
      let workersComp_2019 = this.workersComp.netCaptivePremium * this.workersComp.expenseDifferences['2019']
      let otherCoverages_2019 = this.otherCoverages.netCaptivePremium * this.otherCoverages.expenseDifferences['2019']

      this.totalExpenseDifferences.total2016 = ((commercialAutoLiability_2016+otherLiability_2016+commercialProperty_2016+workersComp_2016+otherCoverages_2016)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalExpenseDifferences.total2017 = ((commercialAutoLiability_2017+otherLiability_2017+commercialProperty_2017+workersComp_2017+otherCoverages_2017)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalExpenseDifferences.total2018 = ((commercialAutoLiability_2018+otherLiability_2018+commercialProperty_2018+workersComp_2018+otherCoverages_2018)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalExpenseDifferences.total2019 = ((commercialAutoLiability_2019+otherLiability_2019+commercialProperty_2019+workersComp_2019+otherCoverages_2019)/this.totalInsuredsPremium*100).toFixed(2)
    },
    calculateTotalProfits: function () {
      let commercialAutoLiability_2016 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.totalProfits['2016']
      let otherLiability_2016 = this.otherLiability.netCaptivePremium * this.otherLiability.totalProfits['2016']
      let commercialProperty_2016 = this.commercialProperty.netCaptivePremium * this.commercialProperty.totalProfits['2016']
      let workersComp_2016 = this.workersComp.netCaptivePremium * this.workersComp.totalProfits['2016']
      let otherCoverages_2016 = this.otherCoverages.netCaptivePremium * this.otherCoverages.totalProfits['2016']

      let commercialAutoLiability_2017 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.totalProfits['2017']
      let otherLiability_2017 = this.otherLiability.netCaptivePremium * this.otherLiability.totalProfits['2017']
      let commercialProperty_2017 = this.commercialProperty.netCaptivePremium * this.commercialProperty.totalProfits['2017']
      let workersComp_2017 = this.workersComp.netCaptivePremium * this.workersComp.totalProfits['2017']
      let otherCoverages_2017 = this.otherCoverages.netCaptivePremium * this.otherCoverages.totalProfits['2017']

      let commercialAutoLiability_2018 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.totalProfits['2018']
      let otherLiability_2018 = this.otherLiability.netCaptivePremium * this.otherLiability.totalProfits['2018']
      let commercialProperty_2018 = this.commercialProperty.netCaptivePremium * this.commercialProperty.totalProfits['2018']
      let workersComp_2018 = this.workersComp.netCaptivePremium * this.workersComp.totalProfits['2018']
      let otherCoverages_2018 = this.otherCoverages.netCaptivePremium * this.otherCoverages.totalProfits['2018']

      let commercialAutoLiability_2019 = this.commercialAutoLiability.netCaptivePremium * this.commercialAutoLiability.totalProfits['2019']
      let otherLiability_2019 = this.otherLiability.netCaptivePremium * this.otherLiability.totalProfits['2019']
      let commercialProperty_2019 = this.commercialProperty.netCaptivePremium * this.commercialProperty.totalProfits['2019']
      let workersComp_2019 = this.workersComp.netCaptivePremium * this.workersComp.totalProfits['2019']
      let otherCoverages_2019 = this.otherCoverages.netCaptivePremium * this.otherCoverages.totalProfits['2019']

      this.totalProfits.total2016 = ((commercialAutoLiability_2016+otherLiability_2016+commercialProperty_2016+workersComp_2016+otherCoverages_2016)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalProfits.total2017 = ((commercialAutoLiability_2017+otherLiability_2017+commercialProperty_2017+workersComp_2017+otherCoverages_2017)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalProfits.total2018 = ((commercialAutoLiability_2018+otherLiability_2018+commercialProperty_2018+workersComp_2018+otherCoverages_2018)/this.totalInsuredsPremium*100).toFixed(2)
      this.totalProfits.total2019 = ((commercialAutoLiability_2019+otherLiability_2019+commercialProperty_2019+workersComp_2019+otherCoverages_2019)/this.totalInsuredsPremium*100).toFixed(2)

      this.projectedSavings2019 = (this.totalInsuredsPremium * (this.totalProfits.total2019/100)).toFixed(0)

      console.log("this.projectedSavings2019: "+this.projectedSavings2019)
    },
    fillDataForChart () {
      this.chartData = {
        labels: ['2016', '2017', '2018', 'Projected 2019'],
        datasets: [
          {
            label: 'Underwriting Profit (Loss)',
            backgroundColor: '#9CFBFF',
            data: [this.totalUWProfitLoss.total2016, this.totalUWProfitLoss.total2017, this.totalUWProfitLoss.total2018, this.totalUWProfitLoss.total2019]
          },
          {
            label: 'Expense Differences (Including Unallocated Loss Adj Expenses)',
            backgroundColor: '#FF8F97',
            data: [this.totalExpenseDifferences.total2016, this.totalExpenseDifferences.total2017, this.totalExpenseDifferences.total2018, this.totalExpenseDifferences.total2019]
          },
          {
            label: 'Investment Income On Ins. Transaction',
            backgroundColor: '#FFE291',
            data:[this.totalIncomeOnInsTransaction.total2016,this.totalIncomeOnInsTransaction.total2017,this.totalIncomeOnInsTransaction.total2018,this.totalIncomeOnInsTransaction.total2019]
          }
        ]
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
  .calculator{
    width:500px;
    margin:0 auto 30px;
    padding:30px;
    border:1px solid #efefef;
    border-radius: 5px;
    form{
      div{
        &:not(:first-child){
          margin-bottom:10px;
          display: flex;
          align-items: center;
          justify-content: space-between;
        }
        label{
          font-size: 12px;
          text-transform: uppercase;
          color:#999;
        }
        input{
          -webkit-appearance: none;
          -moz-appearance: none;
          appearance: none;
          outline:0;
          border:1px solid #efefef;
          border-radius: 4px;
          padding:10px;
          position: relative;
        }
        .submit{
          -webkit-appearance: none;
          -moz-appearance: none;
          appearance: none;
          outline:0;
          border-radius: 4px;
          padding:10px;
          position: relative;
          width: 147px;
          font-weight: bold;
          color:white;
          background-color: #2583e3;
          cursor:pointer;
        }
      }
      .form-header{
        h2{
          width:100%;
          text-align: center;
          margin:0 0 15px;
        }
        p{
          font-size: 12px;
          font-style: italic;
          color:#777;
          margin:0 0 20px;
        }
      }
    }
  }
  .results{
    border:1px solid #efefef;
    border-radius: 5px;
    padding:20px;
    width:850px;
    margin:0 auto;
    .total-savings{
      text-align: center;
      margin-bottom: 30px;
      span{
        color: #85bb65;
      }
    }
    .breakdown{
      ul{
        padding-left: 20px;
      }
    }
    .notes{
      font-size: 14px;
      font-style: italic;
    }
  }
</style>
