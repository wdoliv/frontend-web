<script setup lang="ts">
import { LogoType } from '~~/types/common'
import * as api_pb from '~~/proto/api_pb'

const config = useRuntimeConfig()

const logo = useLogo()
if (config.app.logoType) {
  logo.value = config.app.logoType
}

const logoLight = useLogoLight()
if (config.app.logoLight) {
  logoLight.value = config.app.logoLight
}

const logoDark = useLogoDark()
if (config.app.logoDark) {
  logoDark.value = config.app.logoDark
}

const token = useToken()
if (config.app.tokenSymbol) {
  token.value = config.app.tokenSymbol
}

const decimals = useDecimals()
if (config.app.tokenDecimals) {
  decimals.value = config.app.tokenDecimals
}

const isMainnet = useIsMainnet()
const { data } = await useFetch<api_pb.BlockResponse.AsObject>('/api/block/number/0')
if (data.value && data.value.block !== undefined) {
  if (data.value.block.hash === '0xb00f92bfab3766a3c3265c395b49880a6346ec7e4b621da7413ad179e38e2050') {
    isMainnet.value = true
  }
}

const title = ref<string>()
const description = ref<string>()
if (logo.value === LogoType.QAN) {
  title.value = 'EAD | EAD Chain Blockchain Explorer'
  description.value = 'EAD Chain is the first blockchain developed for the online education segment. It brings transparency, security, and global accessibility to educational content, transforming the future of learning through decentralized technology.'
} else {
  title.value = 'EAD | EAD Chain Blockchain Explorer'
  description.value = 'EAD Chain is the first blockchain developed for the online education segment. It brings transparency, security, and global accessibility to educational content, transforming the future of learning through decentralized technology.'
}  

useSeoMeta({
  title: title.value,
  description: description.value
})

useHead({
  htmlAttrs: {
    lang: 'en'
  }
})
</script>

<template>
  <div>
    <NuxtLayout>
      <div class="min-h-screen relative">
        <LoadingIndicator color="#8355eb" />
        <AppHeader />
        <NuxtPage />
        <AppFooter />
      </div>
    </NuxtLayout>
  </div>
</template>
