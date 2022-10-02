<template>
        <div class="pt-5 space-y-6">
      <UIInput
        v-model="data.username"
        label="Username"
        type="text"
        placeholder="@username"
      />

      <UIInput
        label="Password"
        placeholder="********"
        type="password"
        v-model="data.password"
      />

      <UIButton @click="handleLogin" liquid :disabled="isButtonDisabled">
        Login
      </UIButton>
      <ul class="mx-2 my-4 text-gray-500">
        <li class="inline-block mx-2">
          <p>Doesn't have an account?</p>
        </li>
        <li class="inline-block mx-2">
          <a href="#" class="text-blue-500 hover:underline" @click.prevent="handleRegFormToggle"
            >Register</a
          >
        </li>
      </ul>
    </div>
</template>
<script setup>
      const emitter = useEmitter();

    const data = reactive({
  username: "",
  password: "",
  loading: false,
});
async function handleLogin() {
  const { login } = useAuth();
  data.loading = true;
  try {
    await login({
      username: data.username,
      password: data.password,
    });
  } catch (error) {
    console.log(error);
  } finally {
    data.loading = false;
  }
}
const isButtonDisabled = computed(() => {
  return !data.username || !data.password || data.loading;
});

function handleRegFormToggle(){
  emitter.$emit('toggleRegForm')
}
</script>