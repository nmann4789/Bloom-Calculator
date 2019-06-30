<script>
  import { Bar, mixins } from 'vue-chartjs'

  export default {
    extends: Bar,
    mixins: [mixins.reactiveProp],
    props: ["chartdata", "options"],
    mounted() {
      this.renderLineChart();
    },
    computed: {
      goodChartData: function() {
        return this.chartData;
      }
    },
    methods: {
      renderLineChart: function() {
        this.renderChart(
          this.goodChartData,
          {
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
        );
      }
    },
    watch: {
      data: function() {
        this._chart.destroy();
        this.renderLineChart();
      }
    }
  }
</script>