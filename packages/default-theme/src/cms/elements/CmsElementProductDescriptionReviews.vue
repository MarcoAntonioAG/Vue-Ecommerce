<template>
  <div v-if="product">
    <SwProductDescription :product="product" />
    <SwProductTabs
      :product-id="product.id"
      :properties="properties"
      :reviews="reviews"
    />
  </div>
</template>

<script>
import SwProductDescription from "@/components/SwProductDescription.vue"
import SwProductTabs from "@/components/SwProductTabs.vue"
import { useCms } from "@shopware-pwa/composables"
import { computed, inject } from "@vue/composition-api"
import { getProductReviews, getProductProperties } from "@shopware-pwa/helpers"

export default {
  components: { SwProductDescription, SwProductTabs },
  name: "CmsElementProductDescriptionReviews",
  props: {
    content: {
      type: Object,
      default: () => ({}),
    },
  },
  setup(props) {
    const { page } = useCms() // fallback for provide/inject, remove in future
    const cmsPage = inject("cms-page", page)
    const product = computed(() => cmsPage.value?.product)

    const reviews = computed(() =>
      getProductReviews({ product: product.value })
    )
    const properties = computed(() =>
      getProductProperties({ product: product.value })
    )

    return {
      product,
      reviews,
      properties,
    }
  },
}
</script>

<style lang="scss" scoped></style>
