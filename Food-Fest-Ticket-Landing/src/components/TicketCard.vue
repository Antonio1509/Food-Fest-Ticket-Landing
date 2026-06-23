<script setup>
defineProps({
  ticket: {
    type: Object,
    required: true
  }
})

defineEmits(['toggle-favourite'])
</script>

<template>
  <div class="ticket-card" :class="[ticket.type, { 'featured': ticket.isFeatured }]">
    <span v-if="ticket.isFeatured" class="featured-badge">Most Popular</span>
    
    <h3>{{ ticket.name }}</h3>
    <p class="description">{{ ticket.description }}</p>
    
    <div class="price">
      <span class="currency">R</span>{{ ticket.price }}
    </div>

    <ul class="benefits-list">
      <li v-for="(benefit, index) in ticket.benefits" :key="index">
        ✓ {{ benefit }}
      </li>
    </ul>

    <div class="card-actions">
      <button class="buy-btn">Buy Now</button>
      
      <button 
        @click="$emit('toggle-favourite', ticket.id)" 
        :class="{ 'fav-active': ticket.isFavourite }"
        class="fav-btn"
        title="Favourite this tier"
      >
        {{ ticket.isFavourite ? '❤️' : '🤍' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.ticket-card {
  background: var(--card-bg, #ffffff);
  border: 2px solid var(--card-border, #e2e8f0);
  color: var(--text-main, #2d3748);
  border-radius: 12px;
  padding: 24px;
  position: relative;
  display: flex;
  flex-direction: column;
  max-width: 350px;
  width: 100%;
  box-sizing: border-box;
  transition: transform 0.3s cubic-bezier(0.25, 0.8, 0.25, 1), box-shadow 0.3s ease, border-color 0.3s ease;
  backdrop-filter: blur(8px);
}

.ticket-card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 20px 30px var(--shadow-color, rgba(0, 0, 0, 0.15));
}

.ticket-card.featured {
  border-color: #ff6b6b;
  box-shadow: 0 10px 25px rgba(255, 107, 107, 0.15);
}

.featured-badge {
  position: absolute;
  top: -12px;
  right: 20px;
  background: #ff6b6b;
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: bold;
  z-index: 10;
}

.price {
  font-size: 2.5rem;
  font-weight: bold;
  color: var(--text-title, #1a202c);
  margin: 15px 0;
}

.benefits-list {
  list-style: none;
  padding: 0;
  margin: 20px 0;
  text-align: left;
  flex-grow: 1; 
}

.benefits-list li {
  margin-bottom: 8px;
  color: var(--text-main, #4a5568);
}

.card-actions {
  display: flex;
  gap: 10px;
  margin-top: auto;
}

.buy-btn {
  flex-grow: 1;
  background: #ff6b6b;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.1s ease;
}

.buy-btn:hover {
  background: #fa5252;
}

.buy-btn:active {
  transform: scale(0.98);
}

.fav-btn {
  padding: 12px;
  border: 1px solid var(--card-border, #cbd5e1);
  background: var(--card-bg, white);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.fav-btn:hover {
  transform: scale(1.1);
}

.fav-btn.fav-active {
  background: #ffe3e3;
  border-color: #ff6b6b;
  animation: heartPop 0.3s ease-out;
}

@keyframes heartPop {
  0% { transform: scale(1); }
  50% { transform: scale(1.3); }
  100% { transform: scale(1); }
}

.ticket-card.bronze {
  border-color: #cd7f32;
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(205, 127, 50, 0.15) 100%);
}
.app-wrapper.dark-mode .ticket-card.bronze {
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(205, 127, 50, 0.25) 100%);
}
.ticket-card.bronze h3 {
  color: #cd7f32;
}

.ticket-card.silver {
  border-color: #a0aec0;
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(160, 174, 192, 0.15) 100%);
}
.app-wrapper.dark-mode .ticket-card.silver {
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(160, 174, 192, 0.25) 100%);
}
.ticket-card.silver h3 {
  color: #718096;
}

.ticket-card.gold {
  border-color: #d4af37;
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(212, 175, 55, 0.15) 100%);
}
.app-wrapper.dark-mode .ticket-card.gold {
  background: linear-gradient(135deg, var(--card-bg) 60%, rgba(212, 175, 55, 0.25) 100%);
}
.ticket-card.gold h3 {
  color: #d4af37;
}
</style>