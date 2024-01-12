<template>
  <div class="flex gap-10 md:flex-row flex-col">
    <div>
      <h2 class="font-medium">MRR</h2>
      <div>
        <Bar
          :data="MRRChartData"
          :options="{
            responsive: true,
            maintainAspectRatio: false
          }"
        />
      </div>
    </div>
    <div>
      <h2 class="font-medium">Churn Rate</h2>
      <div>
        <Bar
          :data="ChurnRateChartData"
          :options="{
            responsive: true,
            maintainAspectRatio: false
          }"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, defineProps } from 'vue'
import 'chart.js/auto'
import { Bar } from 'vue-chartjs'

type DataMRR = {
  month: string
  mrr: number
}

type DataChurnRateData = {
  month: string
  churnRate: number
}

const props = defineProps<{
  mrrData: DataMRR[]
  churnRateData: DataChurnRateData[]
}>()

const MRRChartData = computed(() => {
  return {
    labels: props.churnRateData?.map((item: DataChurnRateData) => item.month) || [],
    datasets: [
      {
        label: 'Churn Rate',
        backgroundColor: '#22c55e',
        data: props.churnRateData?.map((item: DataChurnRateData) => item.churnRate) || []
      }
    ]
  }
})

const ChurnRateChartData = computed(() => {
  return {
    labels: props.mrrData?.map((item: DataMRR) => item.month) || [],
    datasets: [
      {
        label: 'MRR',
        backgroundColor: '#22c55e',
        data: props.mrrData?.map((item: DataMRR) => item.mrr) || []
      }
    ]
  }
})
</script>
