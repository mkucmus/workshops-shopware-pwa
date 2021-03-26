<template>
  <div>
    <h1>{{ productName }}</h1>
    <SfButton @click="fetchAnotherProduct">fetch another product</SfButton>
    <div v-if="isLoggedIn">
      {{ $t('You are logged in') }}
    </div>
    <div v-else>
      {{ $t('You are not logged in') }}
    </div>
    <SwProductCard :product="product" />
  </div>
</template>

<script>
import { SfBanner, SfButton } from '@storefront-ui/vue'
import SwProductCard from '@/components/SwProductCard'
import { getProduct, getCmsPage } from '@shopware-pwa/shopware-6-client'
import { getProductName } from '@shopware-pwa/helpers'
import {
  getApplicationContext,
  useSessionContext,
  useUser,
  useNotifications,
} from '@shopware-pwa/composables'
import { onMounted, ref, computed } from '@vue/composition-api'

export default {
  components: {
    SfBanner,
    SwProductCard,
    SfButton,
  },

  setup(_, { root }) {
    const { isLoggedIn, login, error } = useUser(root)
    const { pushError, pushWarning } = useNotifications(root)
    const { currency } = useSessionContext(root)
    const { apiInstance } = getApplicationContext(root)
    const productObject = ref(null)
    const productName = computed(() =>
      getProductName({ product: productObject.value })
    )
    const logInDummyUser = async () => {
      const user = {
        username: 'test',
        password: 'test',
      }
      try {
        await login(user)
        if (error.value) {
          pushError(error.value)
          console.error('login', error)
        }
      } catch (err) {}
    }

    const fetchAnotherProduct = async () => {
      const { product } = await getCmsPage(
        'Incredible-Concrete-New-Centurian/e2d4a5050a474b1c847538123b1d1064',
        null,
        apiInstance
      )

      productObject.value = product
    }

    onMounted(async () => {
      const { product } = await getCmsPage(
        'Aerodynamic-Concrete-Crystal-Qleen/a1dbcd78704d4293b179dc46335fdc78',
        null,
        apiInstance
      )

      productObject.value = product
    })

    return {
      product: productObject,
      currency,
      isLoggedIn,
      logInDummyUser,
      productName,
      pushWarning,
      fetchAnotherProduct,
    }
  },

  methods: {
    pushEmptyWarning() {
      this.pushWarning('some warning')
    },
  },
}
</script>
<style lang="scss" scoped></style>
