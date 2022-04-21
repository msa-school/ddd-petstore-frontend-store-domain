<template>
  <v-data-table
    :headers="headers"
    :items="cartItems"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>

</template>

<script>

const axios = require('axios').default;

export default {    // public class App{
  name: 'Cart',

  components: {
  },

  data: () => ({
    headers: [
      { text: '이름', value: 'petDetail.name' },
      { text: '활동성', value: 'petDetail.energy' },
      { text: '외모', value: 'petDetail.appearance' },
      { text: '결재방법', value: 'payment.method' },
      { text: '결재상태', value: 'payment.status' }
    ],

    cartItems: [],
    showCart: false
  }),

  created(){
    this.init();
  },

  methods:{
    showCart(){
      this.showCart = true;
    },

    async init(){
      const response = await axios.get("/cartItems");
      
      this.cartItems = response.data._embedded.cartItems;

      this.cartItems.forEach(async cartItem => {
        const response = await axios.get(new URL(cartItem._links.pet.href).pathname);
        cartItem.petDetail = response.data;
      })
    },

  }
};
</script>
