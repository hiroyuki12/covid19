<template>
  <v-col cols="12" md="6" class="DataCard">
    <agency-bar-chart
      :title="$t('陽性患者の状況の推移')"
      :title-id="'patients-status'"
      :chart-id="'patients-status'"
      :chart-data="agencyData"
      :date="agencyData.date"
      :unit="$t('人')"
    />
  </v-col>
</template>

<script>
import agencyData from '@/data/agency2.json'
import AgencyBarChart from '@/components/AgencyBarChart2.vue'

export default {
  components: {
    AgencyBarChart
  },
  data() {
    return {
      agencyData
    }
  }
}
</script>
