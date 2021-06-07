<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <table>
      <tr>
        <th>ID</th>
        <th>商品名</th>
        <th>値段</th>
      </tr>
      <tr v-for="product in products" :key="product.id">
        <td>{{ product.id }}</td>
        <td>{{ product.name }}</td>
        <td>{{ product.price }}</td>
      </tr>
    </table>
    <br />
    <form>
    <label
        >ID:
        <input type="text" v-model="id" />
      </label>
      <label
        >商品名:
        <input type="text" v-model="name"/>
      </label>
      <label
        >値段:
        <input type="number" v-model="price" />
      </label>
      <button type="button" @click="createProduct">追加</button>
    </form>
  </div>
</template>

<script>
import axios from "axios"; //追記
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      products: [],
      id: "",
      name: "",
      price: null,
    };
  },
  methods: {
    createProduct(){
      console.log(this.id, this.name, this.price);
      let params = new URLSearchParams();
      params.append("id", this.id);
      params.append("name", this.name);
      params.append("price", this.price);
      axios.post("http://localhost:8080/api/product", params)
      .then(res => {
        this.products.push(res.data);
      })
      .catch(err => console.log(err))
      .finally(() => this.formClear());
    },
    formClear(){
      this.id = this.name = this.price = null;
    }
    
  },
  mounted: function(){
     axios
        .get("http://localhost:8080/api/product")
        .then((res) => {
          this.products = res.data;
        })
        .catch((err) => console.log(err));
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
form {
  display: flex;
  flex-direction: column;
  width: 300px;
}
label,
button {
  width: max-content;
}
</style>
