<script setup lang="ts">
import _ from "lodash"
import { computed, onBeforeMount, ref } from 'vue'
import type { SellerInterface } from '@/interfaces/seller.interface'
import { useSellersStore } from '@/stores/seller'
import { useIntersectionObserver } from '@vueuse/core'
import LoadingComponent from '@/components/LoadingComponent.vue'
import SellerCard from '@/components/SellerCard.vue'

const sellersStore = useSellersStore()
const loader = ref<HTMLElement | null>(null);
const sellers = computed((): SellerInterface[] => {
  return sellersStore.getSellersList
})

const loadMore = () => {
  sellersStore.loadMoreSellers(50)
}

onBeforeMount(() => {
  sellersStore.loadMoreSellers(50);
});

// eslint-disable-next-line @typescript-eslint/no-unused-vars
const { stop } = useIntersectionObserver(
  loader,
  ([{isIntersecting}]: any) => {
    if (isIntersecting) {
      setTimeout(() => {
        loadMore()
      }, 1500)
    }
  }
)

</script>

<template>
  <div class="container m-0 p-0">
    <div class="row m-0 p-0">
      <div class="col-12" v-for="(chunk, index) in _.chunk(_.orderBy(sellers, ['rating'], ['desc']), 4)" :key="index">
          <div class="row m-0 justify-content-center">
            <template v-for="(seller, key) of chunk" :key="key">
              <div class="col-12 col-md-6 col-lg-6 col-xl-4 col-xxl-3 col-fhd-3 m-0">
                <div class="p-1">
                  <SellerCard :seller="seller"></SellerCard>
                </div>
              </div>
            </template>
          </div>
      </div>
    </div>
    <div ref="loader">
      <LoadingComponent />
    </div>
  </div>
</template>
