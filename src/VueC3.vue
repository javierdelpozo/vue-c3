<template>
  <div class="vuec3-chart"></div>
</template>

<script>
  import c3 from 'c3'

  export default {
    name: 'vuec3',
    props: {
      // Chart //
      bindto: {
        required: false,
        type: String
      },
      size: {
        required: false,
        type: Object
      },
      padding: {
        required: false,
        type: Object
      },
      color: {
        required: false,
        type: Object
      },
      interaction: {
        required: false,
        type: Object
      },
      transition: {
        required: false,
        type: Object
      },
      oninit: {
        required: false,
        type: Function
      },
      onrendered: {
        required: false,
        type: Function
      },
      onmouseover: {
        required: false,
        type: Function
      },
      onmouseout: {
        required: false,
        type: Function
      },
      onresize: {
        required: false,
        type: Function
      },
      onresized: {
        required: false,
        type: Function
      },
      // Data //
      data: {
        required: true,
        type: Object
      },
      // Axis //
      axis: {
        required: false,
        type: Object
      },
      // Grid //
      grid: {
        required: false,
        type: Object
      },
      // Region //
      regions: {
        required: false,
        type: Array
      },
      // Legend //
      leyend: {
        required: false,
        type: Object
      },
      // Tooltip //
      tooltip: {
        required: false,
        type: Object
      },
      // Subchart //
      subchart: {
        required: false,
        type: Object
      },
      // Zoom //
      zoom: {
        required: false,
        type: Object
      },
      // Point //
      point: {
        required: false,
        type: Object
      },
      // Line //
      line: {
        required: false,
        type: Object
      },
      // Area //
      area: {
        required: false,
        type: Object
      },
      // Bar //
      bar: {
        required: false,
        type: Object
      },
      // Pie //
      pie: {
        required: false,
        type: Object
      },
      // Donut //
      donut: {
        required: false,
        type: Object
      },
      // Gauge //
      gauge: {
        required: false,
        type: Object
      }
    },
    data() {
      return {
        $chart: null,
        options: {
          bindto: null,
          data: null
        }
      }
    },
    computed: {
      isInitialized() {
        return this.$chart ? true : false;
      },
      onDataChange() {
        return this.data ? this.data : null;
      }
    },
    methods: {
      generateChart(options) {
        this.$chart = c3.generate(this.options);
      },
      loadChart() {
        if (this.isInitialized) this.$chart.load(this.data);
      },
      unloadChart() {
        if (this.isInitialized) this.$chart.load(this.data);
      },
      destroyChart () {
        if (this.isInitialized) this.$chart = this.$chart.destroy();
      }
    },
    mounted() {
      this.options =  {...this._props};
      this.options.bindto = this.bindto ? this.bindto : this.$el;
      
      Object.keys(this.options).forEach((key) => {
        if (this.options[key] === null || this.options[key] === undefined) delete this.options[key]
      });

      if (this.handler) {
        this.handler.$on(events.INIT, (options = {}) => {
          this.destroyChart();

          options.bindto = this.$el;
          this.generateChart(options);
        })

        this.handler.$on(events.DESTROY, () => {
          this.destroyChart();
        })

        this.handler.$on(events.DISPATCH, (cb) => {
          if (cb && this.$chart) {
            cb.call(null, this.$chart)
          }
        })
      } else {
        this.$chart = c3.generate(this.options);
      }
    },
    watch: {
      data: function(oldVal, newVal) {
        if (oldVal !== newVal) this.loadChart
      }
    },
    beforeDestroy() {
      this.destroyChart()
    }
  }
</script>

<style>
  @import "../assets/styles/c3/c3.min.css";
</style>

