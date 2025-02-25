<template>
  <div class="MainPage">
    <div class="Header mb-3">
      <page-header :icon="headerItem.icon">
        {{ headerItem.title }}
      </page-header>
      <div class="UpdatedAt">
        <span>{{ $t('最終更新') }} </span>
        <time :datetime="updatedAt">{{ Data.lastUpdate }}</time>
      </div>
      <div
        v-show="!['ja', 'ja-basic'].includes($i18n.locale)"
        class="Annotation"
      >
        <span>{{ $t('注釈') }} </span>
      </div>
    </div>
    <whats-new class="mb-4" :items="homepageNewsItems" />
    <static-info
      class="mb-4"
      href="https://www.pref.okinawa.lg.jp/site/chijiko/kohokoryu/koho/2020_new_korona_virs.html"
      target="_blank"
      rel="noopener noreferrer"
      :text="$t('自分や家族の症状に不安や心配があればまずは電話相談をどうぞ')"
    />
    <v-row class="DataBlock">
      <confirmed-cases-details-card />
      <!-- <tested-cases-details-card /> -->
      <confirmed-cases-attributes-card />
      <confirmed-cases-number-card />
      <!-- <tested-number-card /> -->
      <okinawa-land-map-card />
      <!-- <okinawa-land-dynamic-map-card /> -->

      <!--<telephone-advisory-reports-number-card />
      <consultation-desk-reports-number-card />
      <metro-card />
      <agency-card />
      <shinjuku-visitors-card />
      <chiyoda-visitors-card /> -->
    </v-row>
    <v-divider />
    <!-- <v-row class="DataBlock">
      <shinjuku-st-map-card />
      <tokyo-st-map-card />
    </v-row> -->
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { MetaInfo } from 'vue-meta'
import PageHeader from '@/components/PageHeader.vue'
import WhatsNew from '@/components/WhatsNew.vue'
import StaticInfo from '@/components/StaticInfo.vue'
import Data from '@/data/data.json'
import News from '@/data/news.json'
import OkinawaLandMapCard from '@/components/cards/OkinawaLandMapCard.vue'
import OkinawaLandDynamicMapCard from '@/components/cards/OkinawaLandDynamicMapCard.vue'
import ConfirmedCasesDetailsCard from '@/components/cards/ConfirmedCasesDetailsCard.vue'
// import TestedCasesDetailsCard from '@/components/cards/TestedCasesDetailsCard.vue'
import ConfirmedCasesNumberCard from '@/components/cards/ConfirmedCasesNumberCard.vue'
import ConfirmedCasesAttributesCard from '@/components/cards/ConfirmedCasesAttributesCard.vue'
import TestedNumberCard from '@/components/cards/TestedNumberCard.vue'
// import InspectionPersonsNumberCard from '@/components/cards/InspectionPersonsNumberCard.vue'
// import TelephoneAdvisoryReportsNumberCard from '@/components/cards/TelephoneAdvisoryReportsNumberCard.vue'
// import ConsultationDeskReportsNumberCard from '@/components/cards/ConsultationDeskReportsNumberCard.vue'
// import MetroCard from '@/components/cards/MetroCard.vue'
// import AgencyCard from '@/components/cards/AgencyCard.vue'
import { convertDatetimeToISO8601Format } from '@/utils/formatDate'
// import ShinjukuVisitorsCard from '@/components/cards/ShinjukuVisitorsCard.vue'
// import ChiyodaVisitorsCard from '@/components/cards/ChiyodaVisitorsCard.vue'
// import ShinjukuStMapCard from '@/components/cards/ShinjukuStMapCard.vue'
// import TokyoStMapCard from '@/components/cards/TokyoStMapCard.vue'

export default Vue.extend({
  components: {
    PageHeader,
    WhatsNew,
    StaticInfo,
    ConfirmedCasesDetailsCard,
    OkinawaLandMapCard,
    OkinawaLandDynamicMapCard,
    // TestedCasesDetailsCard,
    ConfirmedCasesNumberCard,
    ConfirmedCasesAttributesCard,
    TestedNumberCard
    // InspectionPersonsNumberCard,
    // TelephoneAdvisoryReportsNumberCard,
    // ConsultationDeskReportsNumberCard,
    // MetroCard,
    // AgencyCard,
    // ShinjukuVisitorsCard,
    // ChiyodaVisitorsCard,
    // ShinjukuStMapCard,
    // TokyoStMapCard
  },
  data() {
    const data = {
      Data,
      headerItem: {
        icon: 'mdi-chart-timeline-variant',
        title: this.$t('県内の最新感染動向')
      },
      newsItems: News.newsItems
    }
    return data
  },
  computed: {
    updatedAt() {
      return convertDatetimeToISO8601Format(this.$data.Data.lastUpdate)
    },
    homepageNewsItems() {
      return this.$data.newsItems.filter(function(item: any) {
        return item.homepage
      })
    }
  },
  head(): MetaInfo {
    return {
      title: this.$t('県内の最新感染動向') as string
    }
  }
})
</script>

<style lang="scss" scoped>
.MainPage {
  .Header {
    display: flex;
    flex-wrap: wrap;
    align-items: flex-end;

    @include lessThan($small) {
      flex-direction: column;
      align-items: baseline;
    }
  }

  .UpdatedAt {
    @include font-size(14);

    color: $gray-3;
    margin-bottom: 0.2rem;
  }

  .Annotation {
    @include font-size(12);

    color: $gray-3;
    @include largerThan($small) {
      margin: 0 0 0 auto;
    }
  }
  .DataBlock {
    margin: 20px -8px;

    .DataCard {
      @include largerThan($medium) {
        padding: 10px;
      }

      @include lessThan($small) {
        padding: 4px 8px;
      }
    }
  }
}
</style>
