<template>
  <div class="container">
  <h1>Hello world</h1>
  <!-- <div v-html="property"></div> -->
   <div id="app">
    <div v-if="error">
      {{ error }}
    </div>

    <div v-else>
      <h1> My items from Strapi: </h1>
      
      <li v-for="items in items" :key="items.id">
        {{ items.Item }}
      </li>
  
    </div>
    </div> 
  </div>
</template>

<script>


export default {
   name: 'App',
  data () {
    return {
      items: [],
      error: null,
      headers: {'Content-Type': 'application/json'}
    }
  },
  methods: {
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    }
  },
  async mounted () {
    try {
      const response = await fetch("http://localhost:1337/items", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
        .then(this.parseJSON);
        this.items = response
    } catch (error) {
      this.error = error
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  text-align: center;
}
 
 li {
   font-size: 24px;
 }



</style>
