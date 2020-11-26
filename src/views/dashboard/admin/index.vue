<template>
  <div class="dashboard-editor-container">
    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>
    <panel-group @handleSetLineChartData="handleSetLineChartData" />
  </div>
</template>

<script>
import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'
import {config_data} from './components/PanelGroup'
import { fetchAIindex } from '@/api/article'

const aiindex = []
const alarms_smoke = []
const alarms_gas = []
const alarms_humidity = []
const alarms_temp = []
const alarms_lift = []

export default {
  name: 'DashboardAIindex',
  components: {
    PanelGroup,
    LineChart
  },
  data() {
    return {
      lineChartData: this.GetLineData()
    }
  },

  methods: {
    handleSetLineChartData(type) {
      this.lineChartData = lineChartData[type]
    },
    
    GetLineData() {
      return {
        aiindexs: aiindex,
        alarms_smokes: alarms_smoke,  
        alarms_gass: alarms_gas, 
        alarms_lifts: alarms_lift, 
        alarms_humiditys: alarms_humidity, 
        alarms_temps: alarms_temp
      };
    },

    getData() {
      fetchAIindex().then(response => {
        aiindex.push(response.data.aiindex)
        alarms_smoke.push(response.data.alarms_smoke)
        alarms_gas.push(response.data.alarms_gas)
        alarms_lift.push(response.data.alarms_lift)
        alarms_humidity.push(response.data.alarms_humidity)
        alarms_temp.push(response.data.alarms_temp)
      })
    }
  },

  created() {
    this.interval = setInterval(() => this.getData(), 10000);
  }

}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
