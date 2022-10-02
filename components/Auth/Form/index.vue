<template>
  <div class="w-full">
    <div class="flex justify-center">
      <div class="w-10 h-10">
        <LogoTwitter />
      </div>
    </div>

    <AuthFormLog  v-if="formToggle == false" />
    <AuthFormReg v-else />
    </div>
</template>
<script setup>
  
const formToggle = ref(false);
const emitter = useEmitter();

emitter.$on("toggleRegForm", () => {
  formToggle.value = !formToggle.value
});
</script>
