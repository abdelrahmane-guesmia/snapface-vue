<template>
  <div>
    <div v-for="item in FruitsItems" :key="item.name" class="Fruit">
      <div v-if="item.categorie === 'Fruits'" class="items">
        {{ item.name }} : {{ item.price }} € => {{ findDiscountValue(item) }} % de remise.
        Prix apres remise => {{ applyDiscountValue(findDiscountValue(item), item.price) }} €
      </div>
    </div>
    <div class="TotalPriceFruits">
      Total Fruits : {{ toFixedNumber(TotalPriceFruits, 2) }} €
    </div>
    <div v-for="item in ClothingItems" :key="item.name" class="Clothing">
      <div v-if="item.categorie === 'Clothing'" class="items">
        {{ item.name }} : {{ item.price }} € => {{ findDiscountValue(item) }} % de remise
        Prix apres remise => {{ applyDiscountValue(findDiscountValue(item), item.price) }} €
      </div>
    </div>
    <div class="TotalPriceClothing">
      Total Clothing : {{ toFixedNumber(TotalPriceClothing, 2) }} €
    </div>
    <div class="Total">
      Total du panier : {{ toFixedNumber(TotalPrice, 2) }} €
    </div>
  </div>
</template>

<script lang="ts">

import { defineComponent, PropType } from 'vue'
import { MultiStoreItem } from 'models/items'
import { DiscountItem } from 'models/discount'

export default defineComponent({
  props: {
    items: {
      type: Array as PropType<MultiStoreItem[]>,
      required: true
    },
    discount: {
      type: Array as PropType<DiscountItem[]>,
      required: true
    }
  },
  computed: {
    FruitsItems(): MultiStoreItem[] {
      return this.items.filter((item: MultiStoreItem) => item.categorie === 'Fruits')
    },
    ClothingItems(): MultiStoreItem[] {
      return this.items.filter((item: MultiStoreItem) => item.categorie === 'Clothing')
    },
    TotalPriceFruits(): number {
      return this.FruitsItems.reduce((acc: number, currentValue: MultiStoreItem) =>
        acc + this.applyDiscountValue(this.findDiscountValue(currentValue), currentValue.price), 0);
    },
    TotalPriceClothing(): number {
      return this.ClothingItems.reduce((acc: number, currentValue: MultiStoreItem) =>
        acc + this.applyDiscountValue(this.findDiscountValue(currentValue), currentValue.price), 0);
    },
    TotalPrice(): number {
      return this.TotalPriceFruits + this.TotalPriceClothing;
    }
  },
  methods: {
    findDiscountValue(StoreItem: MultiStoreItem): number {
      const discountValue: DiscountItem | undefined = this.discount.find((item: DiscountItem) =>
        ((item.name === StoreItem.name) && (item.categorie === StoreItem.categorie))
      )
      return discountValue ? discountValue.percentage : 0;
    },
    applyDiscountValue(discount: number, price: number): number {
      return price * (1 - discount / 100)
    },
    toFixedNumber(num: number, toFixTo: number = 2, base: number = 10): number {
      const pow = Math.pow(base, toFixTo)
      return +(Math.round(num * pow) / pow)
    }
  }
})
</script>