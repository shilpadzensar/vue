<template>

  <div class="search">

    <h2>Filter LearnVue Article</h2>
    <div>
      <input type="text" v-model.trim="search" @keyup="getAllStarWarsproducts" placeholder="Filter Search">
    </div>

    <div>
      <button @click="reset()">Reset</button>
    </div>

    <div class="result-msg" v-if="search && products.length > 0">
      Showing {{ products.length }} results for "{{ this.search }}"
    </div>

    <div v-if="search && !products.length > 0">
      <p>No results found!</p>
    </div>

    <div class="result" v-if="search && products.length > 0">
      <p v-for="product in products" :key="product.id">
        {{ product.title }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "AutoSearch",

  data() {
    return {
      products: [],
      search: ""
    };
  },

  mounted() {
    console.log("Component mounted.");
  },

  methods: {
    getAllStarWarsproducts() {
      fetch(" http://localhost:3000/products")
        .then(response => response.json())
        .then(res => {
          if (this.search) {
            this.products = res.filter(products =>
              products.title.toLowerCase().includes(this.search.toLowerCase())
            );
          } else {
            this.products = [];
          }
        });
    },
    reset() {
      this.products = [];
    }
  },
  created() {
    this.getAllStarWarsproducts();
  }
};
</script>

<style scoped>
.search {
  margin: 0 430px;
}

input {
  border: 2px solid grey;
  width: 150px;
  height: 20px;
  outline: none;
  border-radius: 4px;
  margin-bottom: 25px;
  padding: 8px;
}

input:hover,
input:focus-visible {
  border: 2px solid darkblue;
}

button {
  font-size: 14px;
  margin-bottom: 20px;
  padding: 5px 10px;
}


.result {
  margin-top: 30px;
  border: 1px solid rgba(0, 0, 0, .125);
  border-radius: 5px;
}

.result p {  
  padding: 20px;
  margin: 0;
}

.result p:not(:last-child){
    border-bottom: 1px solid rgba(0, 0, 0, .125);
}

.result-msg {
  float: right;
  font-size: 12px;
  color: grey;
}
</style>
