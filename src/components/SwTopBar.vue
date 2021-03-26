<template>
  <SfTopBar
    :key="$route.path"
    class="sw-top-bar desktop-only"
    data-cy="top-bar"
  >
    <template #left>
      {{ phrase }}
    </template>
    <template #right>
      <SwCurrencySwitcher class="sf-header__currency" />
      <SwLanguageSwitcher />
    </template>
  </SfTopBar>
</template>

<script>
import { SfTopBar } from '@storefront-ui/vue'
import SwCurrencySwitcher from '@/components/SwCurrencySwitcher.vue'
import SwLanguageSwitcher from '@/components/SwLanguageSwitcher.vue'
import { invokePost, invokeGet, getCart } from '@shopware-pwa/shopware-6-client'
import { onMounted, ref, computed } from '@vue/composition-api'
import { getApplicationContext } from '@shopware-pwa/composables'

export default {
  // GET /sales-channel-api/v{version}/random-phrase

  // 1. setup
  // 2. pobieramy z endpointa te wartosci
  // 3. ustawiamy w ref przychodzącą wartość
  components: {
    SfTopBar,
    SwCurrencySwitcher,
    SwLanguageSwitcher,
  },
  props: {
    user: {
      type: Object,
      requred: false,
      default: () => ({}),
    },
  },
  setup({ user }, { root }) {
    const { apiInstance } = getApplicationContext(root)
    const phrase = ref('is loading....')
    onMounted(async () => {
      const response = await invokeGet(
        {
          address: `/sales-channel-api/v3/random-phrase`,
        },
        apiInstance
      )
      const randomPhrase = response.data.phrase

      phrase.value = randomPhrase
    })

    return {
      phrase,
    }
  },
}
</script>

<style lang="scss">
.sf-top-bar__container {
  box-sizing: border-box;
  margin: 0 auto;
  max-width: 1320px;
  padding: 0 1rem;
}
</style>

<style lang="scss" scoped>
@import '@/assets/scss/variables';

.sw-top-bar {
  position: relative;
  z-index: 3;

  &__location-label {
    margin: 0 var(--spacer-sm) 0 0;
  }

  .sf-header {
    &__currency {
      --select-dropdown-z-index: 2;
      position: relative;
      margin: 0 var(--spacer-base) 0 var(--spacer-base);
      width: 2.5rem;
      &::before {
        content: '';
        display: block;
        position: absolute;
        background-color: white;
        width: 20px;
        top: 50%;
        transform: translate(-50%, -50%);
        border-radius: 50%;
        padding: var(--spacer-2xs);
        left: 50%;
        height: 20px;
      }
    }
    &__header {
      padding-left: var(--spacer-sm);
    }
    &__icon {
      --icon-size: 1.25rem;
    }
  }
}
</style>
