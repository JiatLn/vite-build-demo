<script setup lang="ts">
const message = ref<string>('test$$')
const cnt = ref<number>(0)
</script>

<template>
  <div h-full w-full flex="c col">
    <div flex items-start gap-2>
      <FillBankTextarea v-model:value="message" v-model:cnt="cnt" />
    </div>
    <p mt-8 text-gray-400>
      This is a demo page, you can clear it and start coding~ :)
    </p>
  </div>
</template>

<style lang="less" scoped>
</style>
