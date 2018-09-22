<template>
  <div id="app">
    <Registration 
		title="PROMO 24 MOIS"
		header="https://michellebeliveau.com/images/banner.jpg"
		header2="assets/images/header.jpg"
		bg="https://sarahmdean.amtamembers.com/amta/assets/spa-black.jpg"
		bg2="assets/images/bg.jpg"
		referer="massobeliveau_crm_promo_001"
		referer2="fitness_crm_promo_001"
		/>
  </div>
</template>

<script>
import Registration from './components/Registration.vue'

export default {
  name: 'app',
  components: {
	Registration
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
  
  
  
}
</style>
