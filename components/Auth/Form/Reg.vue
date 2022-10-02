<template>
  <div class="pt-5 space-y-6">
    <UIInput
      v-model="data.username"
      label="Username"
      type="text"
      placeholder="@username"
    />
    <UIInput
      v-model="data.email"
      label="Email"
      type="text"
      placeholder="yourmail@example.com"
    />
    <UIInput v-model="data.name" label="Name" type="text" placeholder="name" />

    <UIInput
      label="Password"
      placeholder="********"
      type="password"
      v-model="data.password"
    />

    <UIInput
      label="Repeat password"
      placeholder="********"
      type="password"
      v-model="data.repeatPassword"
    />

    <UIButton @click="handleRegister" liquid :disabled="isButtonDisabled">
      Register
    </UIButton>
    <ul class="mx-2 my-4 text-gray-500">
      <li class="inline-block mx-2">
        <p>Have an account?</p>
      </li>
      <li class="inline-block mx-2">
        <a
          href="#"
          class="text-blue-500 hover:underline"
          @click.prevent="handleRegFormToggle"
          >Log In</a
        >
      </li>
    </ul>
  </div>
</template>
<script setup>
const emitter = useEmitter();

const data = reactive({
  username: "",
  email: "",
  name: "",
  password: "",
  repeatPassword: "",
  loading: false,
});
async function handleRegister() {
  const { register } = useAuth();
  data.loading = true;
  try {
    await register({
      username: data.username,
      name: data.name,
      email: data.email,
      password: data.password,
      repeatPassword: data.repeatPassword,
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

function handleRegFormToggle() {
  emitter.$emit("toggleRegForm");
}
</script>
