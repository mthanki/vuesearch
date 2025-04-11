<script setup lang="ts">
import { ref, computed } from 'vue';

const emailData = [
  { email: "ann@timescale.com", isSelected: false },
  { email: "bob@timescale.com", isSelected: false },
  { email: "brian@qwerty.com", isSelected: true },
  { email: "james@qwerty.com", isSelected: false },
  { email: "jane@awesome.com", isSelected: false },
  { email: "kate@qwerty.com", isSelected: true },
  { email: "mike@hello.com", isSelected: true }
];

const groupedEmails = ref(
  emailData.reduce((acc, { email }) => {
    const domain = email.split('@')[1];
    if (!acc[domain]) acc[domain] = [];
    acc[domain].push(email);
    return acc;
  }, {})
);

const collapsed = ref(
  Object.keys(groupedEmails.value).reduce((acc, domain) => {
    acc[domain] = true;
    return acc;
  }, {})
);

const searchQuery = ref('');
const suggestions = computed(() => {
  if (!searchQuery.value) return [];
  return Object.keys(groupedEmails.value).filter(domain =>
    domain.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const filteredGroupedEmails = computed(() => {
  if (!searchQuery.value || !selectedSuggestion.value) return groupedEmails.value;
  return { [selectedSuggestion.value]: groupedEmails.value[selectedSuggestion.value] };
});

const selectedSuggestion = ref(null);

function selectSuggestion(domain) {
  selectedSuggestion.value = domain;
  searchQuery.value = domain;
  Object.keys(collapsed.value).forEach(key => {
    collapsed.value[key] = key !== domain;
  });
  suggestions.value = [];
}

const selectedDomains = ref({});

function moveToSelected(domain) {
  if (groupedEmails.value[domain]) {
    selectedDomains.value[domain] = groupedEmails.value[domain];
    delete groupedEmails.value[domain];
    delete collapsed.value[domain];
  }
}

function moveToAvailable(domain) {
  if (selectedDomains.value[domain]) {
    groupedEmails.value[domain] = selectedDomains.value[domain];
    delete selectedDomains.value[domain];
    collapsed.value[domain] = true;
  }
}
</script>

<template>
  <div class="search-container">
    <input type="text" placeholder="Search..." class="search-bar" v-model="searchQuery" />
    <ul v-if="suggestions.length" class="suggestions-dropdown">
      <li v-for="domain in suggestions" :key="domain" @click="selectSuggestion(domain)">
        {{ domain }}
      </li>
    </ul>
  </div>
  <div class="content-section">
    <div class="content-left">
      <h2>Available</h2>
      <div v-for="(emails, domain) in filteredGroupedEmails" :key="domain" class="collapsible">
        <div class="collapsible-header" @click="collapsed[domain] = !collapsed[domain]">
          <span>{{ domain }}</span>
          <button class="add-button" @click.stop="moveToSelected(domain)">+</button>
        </div>
        <div v-if="!collapsed[domain]" class="collapsible-content">
          <ul>
            <li v-for="email in emails" :key="email">{{ email }}</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="content-right">
      <h2>Selected</h2>
      <div v-for="(emails, domain) in selectedDomains" :key="domain" class="collapsible">
        <div class="collapsible-header" @click="collapsed[domain] = !collapsed[domain]">
          <span>{{ domain }}</span>
          <button class="remove-button" @click.stop="moveToAvailable(domain)">-</button>
        </div>
        <div v-if="!collapsed[domain]" class="collapsible-content">
          <ul>
            <li v-for="email in emails" :key="email">{{ email }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.search-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: auto;
  padding: 1rem 0;
  background-color: #f5f5f5;
  margin: 0;
  position: sticky;
  top: 0;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
}

.search-bar {
  width: 80%;
  max-width: 600px;
  padding: 1rem;
  font-size: 1.2rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  outline: none;
  transition: box-shadow 0.3s ease;
}

.search-bar:focus {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  border-color: #007bff;
}

.suggestions-dropdown {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  max-width: 600px;
  background-color: #ffffff;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  list-style: none;
  padding: 0;
  margin: 0;
  z-index: 1000;
}

.suggestions-dropdown li {
  padding: 0.5rem 1rem;
  cursor: pointer;
}

.suggestions-dropdown li:hover {
  background-color: #f0f0f0;
}

.content-section {
  display: flex;
  width: 100%;
  background-color: #ffffff;
  padding: 1rem 0;
  box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.1);
}

.content-left,
.content-right {
  flex: 1;
  padding: 1rem;
  text-align: center;
  border: 1px solid #ddd;
}

.content-left {
  border-right: none;
}

h2 {
  margin: 0;
  font-size: 1.5rem;
  color: #333;
}

.collapsible {
  margin: 1rem 0;
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
}

.collapsible-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 1rem;
  background-color: #f0f0f0;
  cursor: pointer;
  font-weight: bold;
}

.collapsible-header:hover {
  background-color: #e0e0e0;
}

.collapsible-content {
  padding: 0.5rem 1rem;
  background-color: #ffffff;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 0.25rem 0;
}

.add-button,
.remove-button {
  width: 32px;
  height: 32px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.add-button {
  background-color: #007bff;
  color: white;
}

.add-button:hover {
  background-color: #0056b3;
}

.remove-button {
  background-color: #dc3545;
  color: white;
}

.remove-button:hover {
  background-color: #c82333;
}
</style>
