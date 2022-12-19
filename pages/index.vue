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
        { name: "Apple", price: 4, categorie: "Fruits" },
        { name: "Pineapples", price: 7, categorie: "Fruits" },
        { name: "Short", price: 2, categorie: "Clothing" },
        { name: "Pants", price: 8, categorie: "Clothing" },
      ],
      discount: [
        { name: "Apple", categorie: "Fruits", percentage: 10 },
        { name: "Pants", categorie: "Clothing", percentage: 15 }
      ]
    };
  }
})
</script>
