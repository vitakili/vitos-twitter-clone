<template>
  <form class="pt-5 space-y-6">
    <UIInput
      v-model="data.username"
      label="Username"
      type="text"
      placeholder="@username"
      :error="data.errors.username"
    />

    <UIInput
      v-model="data.email"
      label="Email"
      type="email"
      placeholder="yourmail@example.com"
      :error="data.errors.email"
    />
    <UIInput
      v-model="data.name"
      label="Name"
      type="text"
      placeholder="name"
      :error="data.errors.name"
    />

    <UIInput
      label="Password"
      placeholder="********"
      type="password"
      v-model="data.password"
      :error="data.errors.password"
    />

    <UIInput
      label="Repeat password"
      placeholder="********"
      type="password"
      v-model="data.repeatPassword"
      :error="data.errors.repeatPassword"
    />

    <UIButton
      @click.prevent="handleRegister"
      liquid
      :disabled="isButtonDisabled"
    >
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
  </form>
</template>
<script setup>
const emitter = useEmitter();

const data = reactive({
  password: "",
  errors: [],
  name: "",
  username: "",
  email: "",
  repeatPassword: "",
  loading: false,
});

const validEmail = (email) => {
  var re =
    /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  return re.test(email);
};

async function handleRegister() {
  if (!data.username) {
    data.errors.username = "Username required.";
  } else {
    data.errors.username = "";
  }
  if (!data.name) {
    data.errors.name = "Name required.";
  } else {
    data.errors.name = "";
  }
  if (!data.email) {
    data.errors.email = "Email required.";
  } else if (!validEmail(data.email)) {
    data.errors.email = "Valid email required.";
  } else {
    data.errors.email = "";
  }

  if (data.password == "") {
    data.errors.password = "Password is required.";
  } else if (data.password.length < 8) {
    data.errors.password = "Password must be 8 characters long.";
  } else {
    data.errors.password = "";
  }
  if (data.repeatPassword !== data.password) {
    data.errors.repeatPassword = "Passwords do not match.";
  } else {
    data.errors.repeatPassword = "";
  }
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
    data.errors;
  } finally {
    data.loading = false;
  }
}

const isButtonDisabled = computed(() => {
  console.log(data.errors);
  return !data.username || !data.password || data.loading || !data.errors;
});

function handleRegFormToggle() {
  emitter.$emit("toggleRegForm");
}
</script>
