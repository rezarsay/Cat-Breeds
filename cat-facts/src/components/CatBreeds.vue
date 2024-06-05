<template>
    <div>
      <h1>Cat Breeds</h1>
      <RandomFact />
      <br>
      <div class="search-container">
        <input v-model="searchQuery" type="text" placeholder="Search for a breed..." />
        <button @click="searchBreeds">Search</button>
      </div>
      <div class="breeds-container">
        <div v-for="(breed,index) in apiBreeds" :key="index" class="breed-card">
          <h2>{{ breed.breed }}</h2>
          <p><strong>Country:</strong> {{ breed.country }}</p>
          <p><strong>Origin:</strong> {{ breed.origin }}</p>
          <p><strong>Coat:</strong> {{ breed.coat }}</p>
          <p><strong>Pattern:</strong> {{ breed.pattern }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import RandomFact from './RandomFact.vue';
  
  export default {
    components: {
      RandomFact,
    },
    data() {
      return {
        breeds: [],
        searchQuery: '',
        apiBreeds: [],
      };
    },
    created() {
      this.getBreeds();
    },
    methods: {
      async getBreeds() {
        try {
          const response = await axios.get('https://catfact.ninja/breeds');
          this.breeds = response.data.data;
          this.apiBreeds = this.breeds;
        } catch (error) {
          console.error('Error fetching breeds:', error);
        }
      },
      searchBreeds() {
        this.apiBreeds = this.breeds.filter(breed =>
          breed.breed.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      },
    },
  };
  </script>
  
  <style scoped>
  h1 {
    color: #333;
  }
  
  .search-container {
    margin-bottom: 20px;
  }
  
  .search-container input {
    padding: 10px;
    width: 200px;
    margin-right: 10px;
  }
  
  .search-container button {
    padding: 10px 15px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .breeds-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .breed-card {
    background: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 10px;
    padding: 20px;
    width: 250px;
    text-align: left;
  }
  
  .breed-card h2 {
    margin-top: 0;
    color: #35495e;
  }
  
  .breed-card p {
    margin: 5px 0;
  }
  </style>
  