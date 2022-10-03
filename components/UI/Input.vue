<template>
  <div>
    <label
      for=""
      v-if="props.label"
      class="block pl-3 ml-px text-sm font-medium text-gray-700"
      >{{ props.label }}</label
    >
    <input
      :value="props.modelValue" @input="event => emits('update:modelValue', event.target.value)"
      class="block px-4 w-full rounded-full shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm focus:bg-blue-50"
      :type="props.type"
      :class="[props.error ? 'border-red-500 bg-red-100': 'border-gray-300']"
      :placeholder="props.placeholder"
    />
  </div>
  <div v-if="props.error"
    class="p-2 mb-2 text-sm text-red-700 bg-red-100 rounded-lg dark:bg-red-200 dark:text-red-800"
    role="alert"
  >
    {{ props.error }}
  </div>
</template>

<script setup>
    const emits = defineEmits(['update:modelValue'])

const props = defineProps({
  modelValue: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    default: null,
  },
  type: {
    type: String,
    default: ''
  },
  placeholder: {
    type: String,
    default: null
  },
  error: {
    type: String,
    default: ''
  }
});
console.log(props.errors)
</script>
