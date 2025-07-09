<template>
  <div class="sidebar">
    <div class="logo"><i class="fas fa-graduation-cap"></i><h1>EduManage</h1></div>
    <div class="nav-menu">
        <div class="nav-item" :class="{active: activeMainTab === 'dashboard' && !currentManagedCompanyId}" @click="navigate('main', 'dashboard')">
            <i class="fas fa-home"></i><span>Dashboard</span>
        </div>
        <div class="nav-item" :class="{active: activeMainTab === 'companies' && !currentManagedCompanyId}" @click="navigate('main', 'companies')">
            <i class="fas fa-building"></i><span>Companies</span>
        </div>
        
        <template v-if="currentManagedCompanyId">
            <hr style="border-color: rgba(255,255,255,0.1); margin: 15px 0;">
            <div class="nav-item" :class="{'active': activeCompanyTab === 'summary'}" @click="navigate('company', 'summary')"><i class="fas fa-info-circle"></i><span>Summary</span></div>
            <div class="nav-item" :class="{'active': activeCompanyTab === 'trainees'}" @click="navigate('company', 'trainees')"><i class="fas fa-users"></i><span>Trainees</span></div>
            <div class="nav-item" :class="{'active': activeCompanyTab === 'settings'}" @click="navigate('company', 'settings')"><i class="fas fa-cog"></i><span>Settings</span></div>
        </template>
        
        <div class="nav-item" @click="$emit('logout')" style="margin-top: auto;"><i class="fas fa-sign-out-alt"></i><span>Logout</span></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    activeMainTab: String,
    currentManagedCompanyId: String,
    activeCompanyTab: String
  },
  methods: {
      navigate(type, tab) {
          this.$emit('navigate', { type, tab });
      }
  }
}
</script>

<style scoped>
/* ... نفس الاستايلات ... */
</style>