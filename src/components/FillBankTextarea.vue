<script setup lang="ts">
import { formatBlank } from '@/utils'

const props = defineProps<{
  value: string
  cnt: number
}>()

const emit = defineEmits<{
  (event: 'update:value', val: string): void
  (event: 'update:cnt', val: number): void
}>()

const textareaRef = ref<HTMLInputElement | null>()

watch(() => props.value, (val) => {
  let cnt = 0
  for (let i = 0; i < val.length - 1; i++) {
    if (val[i] === '$' && val[i + 1] === '$') {
      cnt++
      i++
    }
  }
  emit('update:cnt', cnt)
})

function onTab() {
  const textarea = textareaRef.value!
  const val: string = textarea.value
  const start = textarea.selectionStart!
  const end = textarea.selectionEnd!
  textarea.value = `${val.substring(0, start)}$$${val.substring(end)}`

  emit('update:value', textarea.value)
}

function onDelete(e: Event) {
  const textarea = textareaRef.value!
  const val: string = textarea.value
  const start = textarea.selectionStart!
  if (val.slice(start - 2, start) === '$$') {
    textarea.value = `${val.substring(0, start - 2)}${val.substring(start + 2)}`
    e.preventDefault()
  }
}
</script>

<template>
  <div w-full>
    <div flex="~ gap-10px" items-center mb-2 ml-2>
      <div text="gray-500">
        按 Tab键 插入填空。
      </div>
    </div>
    <textarea
      ref="textareaRef"
      w-full outline-none rounded px-2 py-1 placeholder:text-gray-400 transition="all duration-600" focus:border="#1890ff" border="~ gray-300"
      :rows="4" placeholder="请输入内容"
      @keyup="emit('update:value', ($event.target as HTMLInputElement).value)"
      @keydown.tab.prevent="onTab"
      @keydown.delete="onDelete"
      v-text="props.value"
    />
    <textarea
      v-show="props.value.length" w-full rounded px-2 py-1 outline-none
      border-none :rows="4" placeholder="请输入题干"
      readonly
      v-text="formatBlank(props.value)"
    />
  </div>
</template>

<style scoped lang="scss"></style>

