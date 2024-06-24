<script setup lang="ts">
import _ from "lodash"
import { colorToHex } from '@/composables/useSellerGenerator'
import { mdiStarOutline } from '@mdi/js'
import { type PropType } from 'vue'
import type { SellerInterface } from '@/interfaces/seller.interface'
import SellerAvatarIcon from '@/components/icons/SellerAvatarIcon.vue'
// @ts-ignore
import SvgIcon from '@jamescoyle/vue-icon'

defineProps({
  seller: {
    type: Object as PropType<SellerInterface>,
    required: true
  }
})
const highlightSeller = (event: any) => {
  document.querySelectorAll('.card').forEach((el) => el.classList.remove('highlight'))

  let element = event.target;

  while (element && !element.classList.contains('card')) {
    element = element.parentElement;
  }

  if (element) {
    element.classList.toggle('highlight');
  }
}
</script>

<template>
  <div @click="highlightSeller" class="card">
    <div class="card-header">
      <div class="row">
        <div class="col-4">
          <div class="d-flex flex-column">
            <span>Sales</span>
            <div>
              <span>{{ seller.sales }}</span>
            </div>
          </div>
        </div>
        <div class="col-4">
          <SellerAvatarIcon :color="colorToHex(seller.color)"/>
        </div>
        <div class="col-4">
          <div class="d-flex flex-column align-items-center">
            <span>Rating</span>
            <div class="d-flex align-items-center justify-content-between">
              <span>{{ seller.rating }}</span>
              <svg-icon type="mdi" :path="mdiStarOutline"></svg-icon>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="card-body">
      <strong>Seller Name:&nbsp;</strong>
      <router-link :to="`/${_.kebabCase(seller.name)}?rating=${seller.rating}&sales=${seller.sales}`">{{ seller.name }}</router-link>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import "@/assets/variables";

.highlight {
  outline: $outline-width $outline-style $outline-color !important;
}
</style>