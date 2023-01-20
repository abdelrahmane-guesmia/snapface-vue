<template>
  <MultiStore :items="items" :discount="discount" />
</template>

<script lang="ts">
import Vue from 'vue'
import MultiStore from '~/components/MultiStore.vue';

export default Vue.extend({
  name: "IndexPage",
  components: { MultiStore },
  data() {
    return {
      items: [
        { name: "Apple", price: 5.5, categorie: "Fruits" },
        { name: "Pineapples", price: 8.5, categorie: "Fruits" },
        { name: "Orange", price: 20.5, categorie: "Fruits" },
        { name: "Short", price: 15, categorie: "Clothing" },
        { name: "Pants", price: 60, categorie: "Clothing" },
      ],
      discount: [
        { name: "Apple", categorie: "Fruits", percentage: 75 },
        { name: "Pants", categorie: "Clothing", percentage: 25 },
        { name: "Short", categorie: "Clothing", percentage: 10 }
      ]
    };
  }
})
</script>
