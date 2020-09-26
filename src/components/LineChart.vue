
<script>
import { Line } from "vue-chartjs";
export default {
  extends: Line,
  props: {
    label: {
      type: String
    },
    chartDate: {
      type: Array
    },
    options: {
      type: Object
    },
    chartColors: {
      type: Object
    }
  },
  data: () => ({}),

  mounted() {
    const dates = this.chartDate.map(d => d.date);
    const totals = this.chartDate.map(d => d.total);

    this.renderChart(
      {
        labels:dates,
        datasets: [
          {
            label: "",
            data: totals,
            borderColor: this.chartColors.borderColor,
            pointBorderColor: this.chartColorspointBorderColor,
            pointBackgroundColor: this.chartColorspointBackgroundColor,
            backgroundColor: this.chartColors.backgroundColor
          }
        ]
      },
      this.options
    );
  }
};
</script>