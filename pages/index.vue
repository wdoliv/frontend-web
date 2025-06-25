<script setup lang="ts">
import * as api_pb from '~~/proto/api_pb'
import { LogoType } from '~~/types/common'

const logo = useLogo()
const latestBlocks = ref<api_pb.Block.AsObject[] | null>(null)
const latestTxs = ref<api_pb.Transaction.AsObject[] | null>(null)

const { data: blocks } = await useFetch<api_pb.ListBlocksResponse.AsObject>('/api/blocks?page=1&limit=7')
if (blocks.value) {
  latestBlocks.value = blocks.value.blocksList
}

const { data: txs } = await useFetch<api_pb.TransactionsListResponse.AsObject>('/api/transactions?limit=6')
if (txs.value) {
  latestTxs.value = txs.value.transactionsList
}
</script>

<template>
  <main>
    <section class="bg-qan-black-900 pt-12 pb-24">
      <div class="container">
        <div class="text-center">
          <h1 class="text-white mb-2">{{ logo === LogoType.EAD ? 'EAD' : 'EAD Chain' }} Blockchain Explorer</h1>
          <Search />
          <div class="qan pt-10 max-lg:pt-5">
            <a class="inline-flex items-center" href="https://eadchain.com" target="_blank">
              <span class="text-white mr-2">by</span>
              <img class="h-6" src="@/assets/images/logo-ead.png" alt="EAD Chain" />
            </a>
          </div>
        </div>
      </div>
    </section>

    <section class="pt-0 -mt-10">
      <div class="container">
        <div class="grid grid-cols-2 gap-6 max-lg:grid-cols-1">
          <LatestBlocks :blocks="latestBlocks" />
          <LatestTxs :txs="latestTxs" />
        </div>
      </div>
    </section>

    <DevSection label="Latest blocks" :pre="latestBlocks" />
    <DevSection label="Latest txs" :pre="latestTxs" />
  </main>
</template>
