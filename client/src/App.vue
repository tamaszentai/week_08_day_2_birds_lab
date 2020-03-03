<template>
  <div id="app">
    <sightings-form />
    <sightings-grid :sightings="sightings" />
  </div>
</template>

<script>
import { eventBus } from '@/main.js';
import SightingsForm from './components/SightingsForm';
import SightingsGrid from './components/SightingsGrid';
import SightingService from './services/SightingService.js';


export default {
  name: 'app',
  components: {
    'sightings-form': SightingsForm,
    'sightings-grid': SightingsGrid
  },
  data(){
    return {
      sightings: []
    }
  },
  mounted(){
    this.fetchData();
    
		eventBus.$on('form-selected', (data) => {
      fetch('http://localhost:3000/api/sightings', {
        method: 'POST',
        body: JSON.stringify(data),
        headers: { 'Content-Type': 'application/json'}
      })
      .then(res => res.json())
      .then(sighting => this.sightings.push(sighting))

    })
	},

  methods: {
    fetchData(){
      SightingService.getSightings()
      .then(sightings => this.sightings = sightings);
    }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  background: url('./assets/birds-background.jpg') no-repeat;
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;

}
</style>
