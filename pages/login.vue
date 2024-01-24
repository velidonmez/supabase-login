<script setup lang="ts">
import type { FormError } from "@nuxt/ui/dist/runtime/types";

const user = useSupabaseUser();
const { auth } = useSupabaseClient();
const redirectTo = `${useRuntimeConfig().public.baseUrl}/confirm`;
const state = ref({
  email: undefined,
  password: undefined,
});

function validate(state: any): FormError[] {
  const errors = [];
  if (!state.email) errors.push({ path: "email", message: "Required" });
  if (!state.password) errors.push({ path: "password", message: "Required" });
  return errors;
}
async function signInWithEmail(form: any) {
  console.log(form);
  const { data, error } = await auth.signInWithPassword({
    email: form.data.email,
    password: form.data.password,
  });
  console.log(data);
  console.log(error);
}

watch(
  user,
  () => {
    if (user.value) return navigateTo("/");
  },
  { immediate: true },
);
</script>

<template>
  <div class="flex flex-col justify-center items-center w-full h-screen">
    <div class="w-[25%]">
      <UForm
        :validate="validate"
        :state="state"
        class="flex flex-col gap-4"
        @submit="signInWithEmail"
      >
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" />
        </UFormGroup>
        <UFormGroup label="Password" name="password">
          <UInput v-model="state.password" type="password" />
        </UFormGroup>
        <UButton type="submit"> Submit </UButton>
      </UForm>
      <UButton
        class="mt-3"
        block
        label="Google"
        color="gray"
        @click="
          auth.signInWithOAuth({ provider: 'google', options: { redirectTo } })
        "
      />
    </div>
  </div>
</template>
