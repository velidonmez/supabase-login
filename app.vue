<script setup lang="ts">
import { withoutTrailingSlash } from "ufo";

const route = useRoute();
const colorMode = useColorMode();

const color = computed(() =>
  colorMode.value === "dark" ? "#18181b" : "white",
);

// Head

useHead({
  meta: [
    { name: "viewport", content: "width=device-width, initial-scale=1" },
    { key: "theme-color", name: "theme-color", content: color },
  ],
  link: [
    { rel: "icon", type: "image/svg+xml", href: "/icon.svg" },
    {
      rel: "canonical",
      href: `https://ui.nuxt.com${withoutTrailingSlash(route.path)}`,
    },
  ],
  htmlAttrs: {
    lang: "en",
  },
});
</script>

<template>
  <div class="flex flex-col">
    <Header />
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <UNotifications>
      <template #title="{ title }">
        <span v-html="title" />
      </template>

      <template #description="{ description }">
        <span v-html="description" />
      </template>
    </UNotifications>
  </div>
</template>
