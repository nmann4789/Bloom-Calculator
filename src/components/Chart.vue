<script>
  import { Bar } from 'vue-chartjs'

  export default {
    extends: Bar,
    props: {
      totalProfits: Object,
      totalExpenseDifferences: Object,
      totalUWProfitLoss: Object,
      totalIncomeOnInsTransaction: Object,
    },
    data () {
      return {
        chartdata: {
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
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            xAxes: [{
              stacked: true,
              categoryPercentage: 0.5,
              barPercentage: 1
            }],
            yAxes: [{
              stacked: true,
            }]
          },
          tooltips: {
            callbacks: {
              label: function(tooltipItem, data) {
                return data['datasets'][tooltipItem['datasetIndex']].label + ": " + tooltipItem.value + '%';
              }
            }
          }
        }
      }
    },
    mounted () {
      this.renderChart(this.chartdata, this.options)
    },
    watch: {
      totalProfits: function(newVal) {
        this.totalProfits = newVal
      },
      totalIncomeOnInsTransaction: function(newVal) {
        this.totalIncomeOnInsTransaction = newVal
      },
      totalUWProfitLoss: function(newVal) {
        this.totalUWProfitLoss = newVal
      },
      totalExpenseDifferences: function(newVal) {
        this.totalExpenseDifferences = newVal
      }
    }
  }
</script>