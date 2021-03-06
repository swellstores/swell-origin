<template>
  <div class="container">
    <!-- Breadcrumb -->
    <NuxtLink
      v-if="previousOrderRoute"
      :to="previousOrderRoute ? previousOrderRoute : '/account/orders/'"
      class="flex items-center cursor-pointer mb-6"
    >
      <BaseIcon icon="uil:angle-left" size="sm" />
      <span class="ml-1">Back to order</span>
    </NuxtLink>

    <div class="mb-16" v-if="accountsSettings">
      <h1 class="text-2xl">{{ accountsSettings.returnsHeading || 'Returns' }}</h1>
      <p
        v-if="accountsSettings.returnsInfoText"
        v-balance-text.children
        class="text-sm"
        v-html="accountsSettings.returnsInfoText"
      ></p>
      <p class="text-sm" v-else>
        We’re sorry that you were unhappy with our product. Contact us at
        <a :href="`mailto:${storeSettings.supportEmail}`">{{ storeSettings.supportEmail }}</a
        >, so we can start the return process. Don’t forget to include the order number and the
        reason for returning. In the meantime take a look at our
        <NuxtLink to="/shipping-returns/">Returns & Polices</NuxtLink> to seek any further
        clarification.
      </p>
    </div>

    <div class="flex">
      <NuxtLink
        v-if="previousOrderRoute"
        to="/account/orders/"
        class="w-full md:w-auto btn light mb-6 md:mr-6"
      >
        Back to all orders
      </NuxtLink>

      <NuxtLink
        to="/products/"
        class="w-full md:w-auto inline-flex justify-center items-center btn dark"
      >
        <BaseIcon icon="uil:shopping-bag" size="sm" class="mr-2" />
        Start shopping
      </NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  fetch() {
    const { $swell } = this

    // Set component data
    this.accountsSettings = $swell.settings.get('customerAccounts')
    this.storeSettings = $swell.settings.get('store', {})
  },

  data() {
    return {
      accountsSettings: {},
      storeSettings: {},
      previousOrderRoute: ''
    }
  },

  watch: {
    // If routing from order, set breadcrumb.
    $route(to, from) {
      if (from.params.id) this.previousOrderRoute = from.path
    }
  },

  mounted() {
    // If routing from order, set breadcrumb.
    if (this.$nuxt.context.from) {
      this.previousOrderRoute = this.$nuxt.context.from.path
    }
  },

  layout: 'account'
}
</script>
