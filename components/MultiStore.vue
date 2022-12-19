<template>
  <div>
    <div v-for="item in FruitsItems" :key="item.name" class="Fruit">
      <div v-if="item.categorie === 'Fruits'" class="items">
        {{ item.name }} : {{ item.price }} € => {{ findDiscountValue(item) }} % de remise.
        Prix apres remise => {{ applyDiscountValue(findDiscountValue(item), item.price) }} €
      </div>
    </div>
    <div class="TotalPriceFruits">
      Total Fruits : {{ TotalPriceFruits }} €
    </div>
    <div v-for="item in ClothingItems" :key="item.name" class="Clothing">
      <div v-if="item.categorie === 'Clothing'" class="items">
        {{ item.name }} : {{ item.price }} € => {{ findDiscountValue(item) }} % de remise
      </div>
    </div>
    <div class="TotalPriceClothing">
      Total Clothing : {{ TotalPriceClothing }} €
    </div>
    <div class="Total">
      Total du panier : {{ TotalPrice }} €
    </div>
  </div>
</template>

<script lang="ts">

import { MultiStoreItem } from 'models/items'
import { DiscountItem } from 'models/discount'

export default {
  props: ["items", "discount"],
  computed: {
    FruitsItems() {
      return this.items.filter((item: MultiStoreItem) => item.categorie === 'Fruits')
    },
    ClothingItems() {
      return this.items.filter((item: MultiStoreItem) => item.categorie === 'Clothing')
    },
    TotalPriceFruits() {
      return this.FruitsItems.reduce((acc: number, currentValue: MultiStoreItem) =>
        acc + this.applyDiscountValue(this.findDiscountValue(currentValue), currentValue.price), 0);
    },
    TotalPriceClothing() {
      return this.ClothingItems.reduce((acc: number, currentValue: MultiStoreItem) =>
        acc + currentValue.price, 0);
    },
    TotalPrice() {
      return this.TotalPriceFruits + this.TotalPriceClothing;
    }
  },
  methods: {
    findDiscountValue(StoreItem: MultiStoreItem): number {
      const discountValue: DiscountItem = this.discount.find((item: DiscountItem) =>
        ((item.name === StoreItem.name) && (item.categorie === StoreItem.categorie))
      )
      if (discountValue)
        return discountValue.percentage
      else
        return 0;
    },
    applyDiscountValue(discount: number, price: number): number {
      return price * (1 - discount / 100)
    }
  }
}
</script>