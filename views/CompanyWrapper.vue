<template>
  <div>
    <div>
      <!-- Navigation Tabs -->
      <button @click="$emit('navigate-company-tab', 'summary')">Summary</button>
      <button @click="$emit('navigate-company-tab', 'trainees')">Trainees</button>
      <button @click="$emit('navigate-company-tab', 'settings')">Settings</button>
    </div>
    <div v-if="activeTab === 'summary'">
      <CompanySummary :companyId="companyId" />
    </div>
    <div v-if="activeTab === 'trainees'">
      <CompanyTrainees :companyId="companyId" />
    </div>
    <div v-if="activeTab === 'settings'">
      <CompanySettings :companyId="companyId" />
    </div>
  </div>
</template>
<script>
import CompanySummary from './company/CompanySummary.vue'
import CompanyTrainees from './company/CompanyTrainees.vue'
import CompanySettings from './company/CompanySettings.vue'
export default {
  name: 'CompanyWrapper',
  props: ['companyId', 'activeTab'],
  components: { CompanySummary, CompanyTrainees, CompanySettings }
};
</script>