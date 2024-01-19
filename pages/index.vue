<script setup lang="ts">
const user = useSupabaseUser();
const { auth } = useSupabaseClient();
async function signOut() {
  await auth.signOut().then(() => {
    navigateTo("/login");
  });
}
</script>

<template>
  <div>
    <UButton @click="signOut"> logout </UButton>
    <h1>Welcome to the homepage</h1>
    <p>{{ user }}</p>
  </div>
</template>
