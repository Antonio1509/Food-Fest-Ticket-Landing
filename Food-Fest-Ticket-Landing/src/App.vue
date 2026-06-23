<script setup>
import { ref, computed } from 'vue'
import TicketCard from './components/TicketCard.vue'

const isDarkMode = ref(false)
const searchQuery = ref('')
const sortBy = ref('default')

const ticketTiers = ref([
  {
    id: 1,
    name: 'Bronze Access',
    type: 'bronze',
    price: 150,
    description: 'Perfect for casual foodies looking to explore.',
    benefits: ['Entry to festival ground', 'Access to street food zone', 'Standard seating'],
    isFeatured: false,
    isFavourite: false,
  },
  {
    id: 2,
    name: 'Silver Experience',
    type: 'silver',
    price: 350,
    description: 'Our most popular choice for dedicated food lovers.',
    benefits: ['All Bronze benefits', '2 Free drink vouchers', 'Access to chef demo stage', 'Priority festival entry'],
    isFeatured: true,
    isFavourite: false
  },
  {
    id: 3,
    name: 'Gold VIP Pass',
    type: 'gold',
    price: 750,
    description: 'The ultimate culinary weekend experience.',
    benefits: ['All Silver benefits', 'Unlimited premium drinks', 'Exclusive VIP lounge access', 'Meet & Greet with head chefs'],
    isFeatured: false,
    isFavourite: false
  }
])

const toggleFavourite = (id) => {
  const ticket = ticketTiers.value.find(t => t.id === id)
  if (ticket) {
    ticket.isFavourite = !ticket.isFavourite
  }
}

const filteredAndSortedTickets = computed(() => {
  let result = ticketTiers.value.filter(ticket => {
    return ticket.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  })

  if (sortBy.value === 'low-to-high') {
    result.sort((a, b) => a.price - b.price)
  } else if (sortBy.value === 'high-to-low') {
    result.sort((a, b) => b.price - a.price)
  }

  return result
})
</script>

<template>
  <div class="app-wrapper" :class="{ 'dark-mode': isDarkMode }">
    <div class="app-container">
      
      <div class="controls-bar">
        <button @click="isDarkMode = !isDarkMode" class="theme-toggle">
          {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
        </button>
      </div>

      <header class="hero-header">
        <h1>Cape Town Food Fest</h1>
        <p>Join us for an unforgettable weekend of food, fun, and community. Explore our ticket choices below!</p>
      </header>

      <div class="filter-section">
        <input 
          v-model="searchQuery" 
          type="text" 
          placeholder="Filter by tier name (e.g. Gold)..." 
          class="search-input"
        />

        <select v-model="sortBy" class="sort-select">
          <option value="default">Sort by: Default</option>
          <option value="low-to-high">Price: Low to High</option>
          <option value="high-to-low">Price: High to Low</option>
        </select>
      </div>

      <main class="tickets-grid">
        <TicketCard 
          v-for="ticket in filteredAndSortedTickets" 
          :key="ticket.id" 
          :ticket="ticket" 
          @toggle-favourite="toggleFavourite"
        />
      </main>

      <p v-if="filteredAndSortedTickets.length === 0" class="no-results">
        No ticket tiers match your criteria. Try adjusting your filter search!
      </p>
    </div>
  </div>
</template>

<style>
.app-wrapper {
  --bg-color: #f7fafc;
  --card-bg: rgba(255, 255, 255, 0.9);
  --card-border: #e2e8f0;
  --text-main: #2d3748;
  --text-title: #1a202c;
  --text-muted: #4a5568;
  --shadow-color: rgba(0,0,0,0.05);

  background-image: url('https://i.ibb.co/mL5ZH82/anand-singha-thumnails-2025-03-21t191821011-2025-03-28f05cd10d0cd0fb31edd835d40172a6.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  min-height: 100vh;
  transition: background-color 0.3s ease;
}

.app-wrapper.dark-mode {
  --bg-color: #121214;
  --card-bg: rgba(30, 30, 36, 0.85);
  --card-border: #2d2d34;
  --text-main: #e2e8f0;
  --text-title: #ffffff;
  --text-muted: #a0aec0;
  --shadow-color: rgba(0,0,0,0.4);

  background-image: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)), url('https://i.ibb.co/mL5ZH82/anand-singha-thumnails-2025-03-21t191821011-2025-03-28f05cd10d0cd0fb31edd835d40172a6.jpg');
}

body {
  margin: 0;
  font-family: 'Segoe UI', Roboto, sans-serif;
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px 20px 60px 20px;
  text-align: center;
}

.controls-bar {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 20px;
}

.theme-toggle {
  background: var(--card-bg);
  color: var(--text-main);
  border: 1px solid var(--card-border);
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
}

.hero-header h1 {
  font-size: 2.8rem;
  color: var(--text-title);
  margin-bottom: 10px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.779);
}

.app-wrapper.dark-mode .hero-header h1 {
  text-shadow: 0 2px 8px rgba(0,0,0,0.6);
}

.hero-header p {
  font-size: 1.2rem;
  color: rgb(11, 11, 11);
  max-width: 600px;
  margin: 0 auto 30px auto;
  font-weight: 500;
}

.filter-section {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 40px;
  flex-wrap: wrap;
}

.search-input, .sort-select {
  padding: 10px 15px;
  border-radius: 8px;
  border: 1px solid var(--card-border);
  background: var(--card-bg);
  color: var(--text-main);
  font-size: 1rem;
}

.search-input {
  width: 300px;
}

.tickets-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 30px;
  justify-items: center;
  align-items: stretch;
  margin-top: 20px;
}

.no-results {
  color: var(--text-muted);
  margin-top: 40px;
  font-style: italic;
}
</style>