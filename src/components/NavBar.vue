<template>
  <nav class="navbar">
    <div class="search-container">
      <input 
        type="text" 
        v-model="searchQuery"
        @keyup.enter="search"
        placeholder="Search for a city..."
        :disabled="loading"
        ref="searchInput"
      >
      <button 
        @click="search" 
        :disabled="loading"
        :class="{ 'loading': loading }"
      >
        <i class="mdi mdi-magnify"></i>
        <span>{{ loading ? 'Searching...' : 'Search' }}</span>
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  props: {
    loading: Boolean
  },
  data() {
    return {
      searchQuery: ''
    }
  },
  methods: {
    search() {
      if (this.searchQuery.trim()) {
        this.$emit('search', this.searchQuery)
      }
    },
    focusInput() {
      this.$refs.searchInput.focus()
    }
  },
  mounted() {
    
    this.focusInput()
    
    
    document.addEventListener('keydown', (e) => {
      if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
        e.preventDefault()
        this.focusInput()
      }
    })
  }
}
</script>