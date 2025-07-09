<template>
  <div id="app">
    <LoginPage v-if="!isLoggedIn" @login-success="handleLogin" />
    <div class="app-container" v-else>
      <Sidebar 
        :activeMainTab="activeMainTab" 
        :currentManagedCompanyId="currentManagedCompanyId"
        :activeCompanyTab="activeCompanyTab"
        @navigate="handleNavigation" 
        @logout="handleLogout" 
      />
      <div class="main-content">
        <div class="header">
          <h2>{{ getPageTitle() }}</h2>
          <div class="user-menu">
            <div class="company-selector" v-if="currentManagedCompanyId">
              <button class="company-selector-btn" @click="showCompanySelectorDropdown = !showCompanySelectorDropdown">
                {{ getCompanyName(currentManagedCompanyId) }} <i class="fas fa-chevron-down"></i>
              </button>
              <div v-if="showCompanySelectorDropdown" class="company-selector-dropdown">
                <div v-for="company in companies" :key="company.id" class="company-selector-item" :class="{'active': company.id === currentManagedCompanyId}" @click="switchManagedCompany(company.id)">
                  {{ company.name }}
                </div>
              </div>
            </div>
            <div class="user-avatar">A</div>
          </div>
        </div>
        
        <Dashboard v-if="!currentManagedCompanyId && activeMainTab === 'dashboard'" :companies="companies" :trainees="trainees" :courses="courses" />
        <Companies v-if="!currentManagedCompanyId && activeMainTab === 'companies'" :companies="companies" @manage-company="selectCompany" />
        
        <CompanyWrapper 
            v-if="currentManagedCompanyId" 
            :companyId="currentManagedCompanyId" 
            :activeTab="activeCompanyTab"
            @navigate-company-tab="path => activeCompanyTab = path"
        />
      </div>
    </div>
  </div>
</template>

<script>
import api from './services/api';
import LoginPage from './views/Login.vue';
import Sidebar from './components/Sidebar.vue';
import Dashboard from './views/Dashboard.vue';
import Companies from './views/Companies.vue';
import CompanyWrapper from './views/CompanyWrapper.vue';

export default {
  name: 'App',
  components: {
    LoginPage,
    Sidebar,
    Dashboard,
    Companies,
    CompanyWrapper
  },
  data() {
    return {
      isLoggedIn: false,
      activeMainTab: 'dashboard',
      activeCompanyTab: 'summary',
      currentManagedCompanyId: null,
      companies: [],
      trainees: [],
      courses: [],
      showCompanySelectorDropdown: false,
    };
  },
  methods: {
    handleLogin() {
      this.isLoggedIn = true;
      this.fetchInitialData();
    },
    handleLogout() {
      this.isLoggedIn = false;
      this.currentManagedCompanyId = null;
    },
    handleNavigation(payload) {
        if (payload.type === 'main') {
            this.activeMainTab = payload.tab;
            this.currentManagedCompanyId = null;
        } else if (payload.type === 'company') {
            this.activeCompanyTab = payload.tab;
        }
    },
    async fetchInitialData() {
      this.companies = await api.getCompanies();
      this.trainees = await api.getAllTrainees();
      this.courses = await api.getAllCourses();
    },
    getCompanyName(companyId) {
        const company = this.companies.find(c => c.id === companyId);
        return company ? company.name : '...';
    },
    selectCompany(companyId) {
        this.currentManagedCompanyId = companyId;
        this.activeCompanyTab = 'summary';
    },
    switchManagedCompany(companyId) {
        this.currentManagedCompanyId = companyId;
        this.activeCompanyTab = 'summary';
        this.showCompanySelectorDropdown = false;
    },
    getPageTitle() {
        if (!this.currentManagedCompanyId) {
            return this.activeMainTab === 'dashboard' ? 'Main Dashboard' : 'Company Management';
        }
        const companyName = this.getCompanyName(this.currentManagedCompanyId);
        return `Managing: ${companyName}`;
    }
  },
  created() {
    // For persistent login simulation
    if (localStorage.getItem('isLoggedIn')) {
        this.isLoggedIn = true;
        this.fetchInitialData();
    }
  }
};
</script>

<style>
/* Global Styles */
/* ... (نفس الاستايلات المذكورة سابقاً) ... */
</style>