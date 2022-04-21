<template>

  <v-card
    class="mx-auto"
    max-width="344"
    outlined
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">
          {{value.type}}
        </div>
        <v-list-item-title class="text-h5 mb-1">
          {{value.name}}
        </v-list-item-title>
        <v-list-item-subtitle >에너지는 {{value.energy}}이고 외모지수는 {{value.appearance}}입니다.</v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="80"
        color="grey"
      ></v-list-item-avatar>
    </v-list-item>

    <v-card-actions>
     
      <v-chip v-if="inTheCart">카드에 담겼습니다</v-chip>
      <v-btn
        v-else
        outlined
        rounded
        text

        @click="addToCart"
      >
        카트담기
      </v-btn>

      <slot name="buttons"></slot>
      
    </v-card-actions>
  </v-card>


</template>

<script>

  const axios = require('axios').default;

  export default {
    name: 'Pet',

    props: {
      value: Object,
      editMode: Boolean
    },

    data: ()=>{
      return {
        inTheCart: false
      }
    },

    methods:{

      async addToCart(){

        var cartItem = {
          pet: this.value._links.self.href,
          customer: "http://localhost:8088/customers/help@uengine.org",
          payment: {
            method: "신용카드",
            status: "결재완료"
          }
        }

        await axios.request({ 
          method: 'POST', 
          url: "/cartItems", 
          headers: {'Content-Type': 'application/json'},
          data: cartItem
        });

        this.inTheCart = true;
      }
    }
  }
</script>
