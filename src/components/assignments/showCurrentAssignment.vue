<template>
  <div> 
    <div>
      <p>Description: {{ order.description }}</p>
      <p>Placed-on: {{ order.placedOn }}</p>
      <p>Pickup: {{ order.fromAddr }}</p>
      <p>Drop: {{ order.toAddr }}</p>
    </div>
      <button v-on:click="fulfillOrder">fulfill</button>
  </div>
</template>

<script>
import runnerMenu from '../menu/runnerMenu.vue'
import vueInstance from '../../views/runner/main.js'

export default {
  components: {
    runnerMenu
  },
  data() {
    return {
      currentOrderUrl: 'http://localhost:8000/runner/currentOrder',
      fulfillOrderUrl: 'http://localhost:8000/runner/fulfillorder',
      order: {},
    }
  },
  methods: {
    async getCurrentAssignments () {
      let fetchedObj = await fetch(this.currentOrderUrl, this.constructFetchBody())
      let order = await fetchedObj.json()
      return order
    },
    constructFetchBody() {
      return {
        mode: 'cors',
        headers: {
          authorization: document.cookie.split(';').map(e=>e.trim()).filter(e=>e.startsWith('access_token='))[0].substring(13),
          'content-type': 'application/json'
        }
      }
    },
    async fulfillOrder() {
      let fetchedObj = await fetch(this.fulfillOrderUrl, {...this.constructFetchBody(), method: 'post', body: JSON.stringify({orderID: this.order._id}) })
      console.log(await fetchedObj.json())
      this.order = await this.getCurrentAssignments()
    }
  },
  async mounted() {
    this.order = await this.getCurrentAssignments()
    console.log(this.order)
  },
}
</script>

<style>

</style>
