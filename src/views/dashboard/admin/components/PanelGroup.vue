<template> 
    <div class="app-container">
        <div class="filter-container">
            <el-select placeholder="Sensor Type" v-model="type_selected" @change="handleType('type',type_selected)">
                <el-option v-for="option in type_options" v-bind:value="option.value">
                    {{ option.label}}
                </el-option>
            </el-select>
            <el-select placeholder="Sensor Num" v-model="num_selected" @change="handleType('num',num_selected)">
                <el-option v-for="option in num_options" v-bind:value="option.value">
                    {{ option.label }}
                </el-option>
            </el-select>
            <el-select placeholder="Sensor Severity" v-model="severity_selected" @change="handleType('severity',severity_selected)">
                <el-option v-for="option in severity_options" v-bind:value="option.value">
                    {{ option.label }}
                </el-option>
            </el-select>
            <el-select placeholder="Failure %" v-model="failure_selected" @change="handleType('failure',failure_selected)">
                <el-option v-for="option in failure_options" v-bind:value="option.value">
                    {{ option.label }}
                </el-option>
            </el-select>
            <el-select placeholder="Refresh" v-model="refresh_selected" @change="handleType('refresh',refresh_selected)">
                <el-option v-for="option in refresh_options" v-bind:value="option.value">
                    {{ option.label }}
                </el-option>
            </el-select>
            <el-button class="filter-item" type="primary" icon="el-icon-plus" @click="handleConfigData">
                Add
            </el-button>
        </div>
        <div>
            <el-table :data="config_list" style="width: 80%;padding-top: 15px;">
                <el-table-column label="Sensor Type" prop="type" min-width="200">
                </el-table-column>
                <el-table-column label="Sensor Num" prop="num" width="200" align="center">
                </el-table-column>
                <el-table-column label="Sensor Severity" prop="severity" width="200" align="center">
                </el-table-column>
                <el-table-column label="Failure %" prop="failure" width="200" align="center">
                </el-table-column>
            </el-table>
  </div>
    </div>
</template>

<script>
import CountTo from 'vue-count-to'

//send config_data to the server
import { updateAIConfig } from '@/api/article' 
export default {

  name: "PanelGroup",

  components: {
    CountTo
  },

  methods: {
    handleSetLineChartData(type) {
      console.log(type)
      this.$emit('handleSetLineChartData', type)
    },

    handleType(key, val) {
      this.config_data[key]=val
    },

    fetchConfigData(config_data){
      if(config_data.refresh>0){
        this.config_list = []
      }
      this.config_list.push({
        type: config_data.type,
        num: config_data.num,
        severity: config_data.severity,
        failure: config_data.failure
      })

    },

    handleConfigData(){
       this.fetchConfigData(this.config_data)
       updateAIConfig(this.config_data).then(response => {
        console.log(response.data)
      })
    }
  },

  data() {
    return {
      config_list: [],
      config_data: {
       type: 1,
       num:200,
       severity:5,
       failure:0.1,
       refresh:0
      },
      aiindex: [],
      severity_selected: undefined,
      severity_options: [{
        value: '9',
        label: 'severity 9'
      }, {
        value: '8',
        label: 'severity 8'
      }, {
        value: '7',
        label: 'severity 7'
      }, {
        value: '6',
        label: 'severity 6'
      }, {
        value: '5',
        label: 'severity 5'
      }, {
        value: '4',
        label: 'severity 4'
      }, {
        value: '3',
        label: 'severity 3'
      }, {
        value: '2',
        label: 'severity 2'
      }, {
        value: '1',
        label: 'servrity 1'
      }],

      type_selected: undefined,
      type_options: [{
        value: '1',
        label: 'Somke'
      }, {
        value: '2',
        label: 'Temperature'
      }, {
        value: '3',
        label: 'Humidity'
      }, {
        value: '4',
        label: 'Lift'
      }, {
        value: '5',
        label: 'Gas'
      }],

      num_selected: undefined,
      num_options: [{
        value: '400',
        label: 'num 400'
      }, {
        value: '1',
        label: 'num 1'
      }, {
        value: '10',
        label: 'num 10'
      }, {
        value: '50',
        label: 'num 50'
      }, {
        value: '100',
        label: 'num 100'
      }, {
        value: '200',
        label: 'num 200'
      }, {
        value: '1000',
        label: 'num 1000'
      }, {
        value: '2000',
        label: 'num 2000'
      }, {
        value: '5000',
        label: 'num 5000'
      }],

      failure_selected: undefined,
      failure_options: [{
        value: '0.01',
        label: 'failure 1%'
      }, {
        value: '0.05',
        label: 'failure 5%'
      }, {
        value: '0.1',
        label: 'failure 10%'
      }, {
        value: '0.15',
        label: 'failure 15%'
      }, {
        value: '0.2',
        label: 'failure 20%'
      }],

      refresh_selected: undefined,
      refresh_options: [{
        value: '0',
        label: 'refresh False'
      }, {
        value: '1',
        label: 'refresh True'
      }],
    }
  }
}

</script>

<style lang="scss" scoped>
.panel-group {
  margin-top: 18px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #fff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, .05);
    border-color: rgba(0, 0, 0, .05);

    &:hover {
      .card-panel-icon-wrapper {
        color: #fff;
      }

      .icon-people {
        background: #40c9c6;
      }

      .icon-message {
        background: #36a3f7;
      }

      .icon-money {
        background: #f4516c;
      }

      .icon-shopping {
        background: #34bfa3
      }
    }

    .icon-people {
      color: #40c9c6;
    }

    .icon-message {
      color: #36a3f7;
    }

    .icon-money {
      color: #f4516c;
    }

    .icon-shopping {
      color: #34bfa3
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 16px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-icon {
      float: left;
      font-size: 48px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        margin-bottom: 12px;
      }

      .card-panel-num {
        font-size: 20px;
      }
    }
  }
}

@media (max-width:550px) {
  .card-panel-description {
    display: none;
  }

  .card-panel-icon-wrapper {
    float: none !important;
    width: 100%;
    height: 100%;
    margin: 0 !important;

    .svg-icon {
      display: block;
      margin: 14px auto !important;
      float: none !important;
    }
  }
}
</style>
