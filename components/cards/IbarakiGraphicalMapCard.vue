<template>
  <v-col cols="12" md="6" class="DataCard">
    <data-view
      :title="$t('区別 陽性者人数マップ(退院含む)')"
      :title-id="'ibaraki-city-map-table'"
      :date="Data.patients.date"
      :source-title="$t('横浜市内の陽性患者の発生状況データ')"
      :source-url="
        $t(
          'https://www.city.yokohama.lg.jp/city-info/koho-kocho/koho/topics/corona-data.html'
        )
      "
    >
      <template v-slot:button>
        <table class="regend-table">
          <tbody>
            <tr>
              <td><span class="color-test infected-level1" />1-5</td>
              <td><span class="color-test infected-level2" />6-10</td>
              <td><span class="color-test infected-level3" />11-15</td>
            </tr>
            <tr>
              <td><span class="color-test infected-level4" />16-20</td>
              <td><span class="color-test infected-level5" />21-30</td>
              <td><span class="color-test infected-level6" />31-</td>
            </tr>
          </tbody>
        </table>
      </template>
      <ibaraki-map />
    </data-view>
  </v-col>
</template>

<script>
import Data from '@/data/data.json'
import IbarakiMap from '@/assets/ibaraki-map.svg'
import DataView from '@/components/DataView.vue'
import CityData from '@/data/cities.json'

export default {
  components: {
    IbarakiMap,
    DataView
  },
  data() {
    const data = {
      Data
    }
    return data
  },
  mounted() {
    const patients = Data.patients.data

    // 市町村の患者人数の連想配列
    const cityPatientsNumber = {}
    for (const key of patients) {
      cityPatientsNumber[key.居住地] = patients.filter(function(x) {
        return x.居住地 === key.居住地
      }).length
    }

    CityData.forEach(element => {
      if (!cityPatientsNumber[element.city]) {
        return
      }
      const targetElement = document.getElementById(
        'ibaraki-map_svg__' + element.Romaji
      )
      if (cityPatientsNumber[element.city] <= 5)
        targetElement.classList.add('infected-level1')
      else if (cityPatientsNumber[element.city] <= 10)
        targetElement.classList.add('infected-level2')
      else if (cityPatientsNumber[element.city] <= 15)
        targetElement.classList.add('infected-level3')
      else if (cityPatientsNumber[element.city] <= 20)
        targetElement.classList.add('infected-level4')
      else if (cityPatientsNumber[element.city] <= 30)
        targetElement.classList.add('infected-level5')
      else targetElement.classList.add('infected-level6')
    })
  }
}
</script>

<style lang="scss" module>
.note {
  margin-top: 10px;
  margin-bottom: 0;
  font-size: 12px;
  color: $gray-3;
}
</style>
<!-- 本来ならばSVGをinline展開してそこに限定してcssを適用するべきだが、inline展開ができなかったため妥協 -->
<style lang="scss">
$infected-level1: #ccfbcc;
$infected-level2: #88f2a9;
$infected-level3: #44e5b7;
$infected-level4: #00c1d5;
$infected-level5: #004da5;
$infected-level6: #000072;

td {
  font-size: 0.9em;
  padding: 0 0 0 10px;
  font-weight: 300;
}

td span {
  padding: 0 0 0 10px;
}

.regend-table {
  margin: auto;
  padding-top: 15px;
}

.color-test {
  vertical-align: middle;
  width: 2rem;
  height: 1rem;
  display: inline-block;
  margin: 0 0.3rem 0 0;
}
// 1-5
.infected-level1 {
  fill: $infected-level1 !important;
  background-color: $infected-level1;
}
// 6-10
.infected-level2 {
  fill: $infected-level2 !important;
  background-color: $infected-level2;
}
// 10-15
.infected-level3 {
  fill: $infected-level3 !important;
  background-color: $infected-level3;
}
// 15-20
.infected-level4 {
  fill: $infected-level4 !important;
  background-color: $infected-level4;
}
// 21-30
.infected-level5 {
  fill: $infected-level5 !important;
  background-color: $infected-level5;
}
// 31-
.infected-level6 {
  fill: $infected-level6 !important;
  background-color: $infected-level6;
}

svg {
  max-height: 600px;
}
</style>
