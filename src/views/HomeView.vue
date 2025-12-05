<template>
  <main>
    <!-- Adiciona o carrossel -->
    <AppCarousel />
    <div class="container py-5">
      <h1>{{ t("homePage.title") }}</h1>
      <p>{{ t("homePage.welcomeMessage") }}</p>
    </div>
  </main>
</template>

<script setup lang="ts">
import { useI18n } from "vue-i18n";
import AppCarousel from "@/components/AppCarousel.vue";

const { t } = useI18n();
</script>
