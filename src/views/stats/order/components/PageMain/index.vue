<template>
  <div class="cs-p">
    <el-card class="box-card" shadow="never">
      <div slot="header" class="box-card-header">
        <span>今日实时</span>
        <span class="update-time">更新时间：{{updateTime}}</span>
      </div>

      <div class="cs-today flex-wrap">
        <div
          v-for="(item, key) in todayData"
          :key="key"
          class="cs-today__block">
          <div class="cs-today__content">
            <div class="cs-today__info">
              <p>{{todayMap[key]}}</p>
              <p class="cs-today__number">{{item}}</p>
            </div>
          </div>
        </div>
      </div>
    </el-card>

    <el-row :gutter="20" class="cs-mt">
      <el-col :span="14">
        <el-card class="box-card" shadow="never">
          <div slot="header" class="box-card-header">
            <span>趋势</span>
          </div>

          <ve-line
            :data="orderData"
            :colors="colors"
            :settings="chartSettings"
            :data-empty="!orderData.rows.length"/>
        </el-card>
      </el-col>

      <el-col :span="10">
        <el-card class="box-card" shadow="never">
          <div slot="header" class="box-card-header">
            <span>订单来源</span>
          </div>

          <ve-pie
            :data="sourceData"
            :colors="colors"
            :data-empty="!sourceData.rows.length"/>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import colors from '@/plugin/careyshop/charts'
import 'v-charts/lib/style.css'

export default {
  components: {
    VeLine: () => import('v-charts/lib/line.common'),
    VePie: () => import('v-charts/lib/pie.common')
  },
  props: {
    todayData: {
      default: () => {}
    },
    orderData: {
      default: () => {}
    },
    sourceData: {
      default: () => {}
    },
    updateTime: {
      default: ''
    }
  },
  data() {
    return {
      colors,
      todayMap: {
        order: '订单量',
        sales: '销售额',
        service: '售后单',
        not_paid: '待付款',
        paid: '已付款',
        not_shipped: '待发货',
        shipped: '已发货',
        not_comment: '待评价'
      },
      chartSettings: {
        labelMap: {
          order: '下单数',
          payment: '付款数',
          percent: '支付率'
        },
        axisSite: {
          right: ['percent']
        },
        yAxisType: ['KMB', 'percent'],
        yAxisName: ['数值', '比率'],
        showLine: ['percent']
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.update-time {
  font-size: 12px;
  color: $color-info;
  float: right;
}

.box-card {
  border-radius: 0;
  border: 1px solid $color-border-1;
}

.box-card-header {
  font-size: 14px;
  color: $color-text-normal;
}

.flex-wrap {
  display: flex;
  flex-wrap: wrap;
  margin: -10px;
}

.cs-today {
  .cs-today__block {
    width: 25%;
    box-sizing: border-box;
  }

  .cs-today__content {
    display: flex;
    color: $color-info;
    border-radius: 4px;
    background-color: #F5F7FA;
    margin: 10px;
    overflow: hidden;
  }

  .cs-today__info {
    width: 100%;
    text-align: center;

    p {
      margin: 10px;
      font-size: 14px;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .cs-today__number {
      color: $color-text-normal;
      font-size: 28px;
    }
  }
}
</style>
