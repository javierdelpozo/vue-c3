<template>
  <div class="vuec3-chart"></div>
</template>

<script>
  import c3 from 'c3'
  import * as events from './events'

  export default {
    name: 'vuec3',
    props: {
      handler: {
        required: true,
        type: Object
      },
      options: {
        bindto: {
          required: false,
          type: String,
          default: this.$el
        },
        size: {
          required: false,
          type: Object
        },
        padding: {
          required: false,
          type: Object
        },
        interaction: {
          retuired: false,
          type: Object
        },
        oninit: {
          required: false,
          type: Function
        },
        data: {
          required: true,
          type: Object
        },
        axis: {
          required: false,
          type: Object
        },
        leyend: {
          required: false,
          type: Object
        },
        tooltip: {
          required: false,
          type: Object
        },
        line: {
          required: false,
          type: Object
        },
        area: {
          required: false,
          type: Object
        },
        bar: {
          required: false,
          type: Object
        },
        pie: {
          required: false,
          type: Object
        }
      }
    },

    data () {
      return {
        $chart: null
      }
    },

    methods: {
      destroyChart () {
        if (this.$chart) {
          this.$chart = this.$chart.destroy()
        }
      }
    },

    mounted () {
      if (this.handler) {

        this.handler.$on(events.INIT, (options = {}) => {
          this.destroyChart()

          options.bindto = this.$el
          this.$chart = c3.generate(options)
        })


        this.handler.$on(events.DESTROY, () => {
          this.destroyChart()
        })


        this.handler.$on(events.DISPATCH, (cb) => {
          if (cb && this.$chart) {
            cb.call(null, this.$chart)
          }
        })

      }

    },

    beforeDestroy () {
      this.destroyChart()
    }
  }
</script>
