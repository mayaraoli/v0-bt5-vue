<template>
  <div class="page-root">
    <header class="topbar">
      <div class="topbar-left">
        <h1 class="site-title">{{ t('coursesPage.pageTitle') }}</h1>
      </div>

      <div class="topbar-right">
        <!-- language selector -->
        <div class="lang-selector" @click="isLanguageOpen = !isLanguageOpen">
          <button class="btn-small">{{ languageLabel }}</button>
          <ul v-if="isLanguageOpen" class="lang-list">
            <li v-for="l in languages" :key="l.code">
              <button class="lang-item" @click="changeLanguage(l.code)">
                {{ languageName(l.code) }}
              </button>
            </li>
          </ul>
        </div>

        <!-- dark mode toggle -->
        <div class="dark-toggle">
          <label>
            <input type="checkbox" v-model="darkMode.value" />
            {{ t('navbar.darkMode') || 'Dark Mode' }}
          </label>
        </div>
      </div>
    </header>

    <main class="cursos-container">
      <section v-for="id in courseIds" :key="id" class="curso">
        <CursoTitulo :titulo="t(`coursesPage.${id}.title`)" />
        <div class="curso-texto">
          <p v-for="(p, i) in t(`coursesPage.${id}.paragraphs`)" :key="i" v-html="p"></p>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";
import { useI18n } from "vue-i18n";
import CursoTitulo from "@/components/CursoTitulo.vue";

// i18n
const { t, locale } = useI18n();

// cursos (ids correspondem às chaves nos JSONs)
const courseIds = [
  "tsi",
  "computacao",
  "civil",
  "bioprocessos",
  "eletronica",
  "matematica",
  "quimicos"
];

// language handling (se você já usa availableLocales no seu setup, pode trocar)
const languages = computed(() => [
  { code: "pt" },
  { code: "en" },
  { code: "es" },
  { code: "fr" }
]);

const isLanguageOpen = ref(false);

const languageName = (code) => {
  const names = {
    pt: "Português",
    en: "English",
    es: "Español",
    fr: "Français"
  };
  return names[code] || code;
};

const languageLabel = computed(() => languageName(locale.value || "pt"));

const changeLanguage = (lang) => {
  locale.value = lang;
  isLanguageOpen.value = false;
  localStorage.setItem("language", lang);
};

// dark mode (reactive)
const darkMode = ref(false);

const applyTheme = () => {
  document.documentElement.setAttribute(
    "data-bs-theme",
    darkMode.value ? "dark" : "light"
  );
  localStorage.setItem("theme", darkMode.value ? "dark" : "light");
};

const detectThemeFromSystem = () => {
  darkMode.value = window.matchMedia("(prefers-color-scheme: dark)").matches;
  applyTheme();
};

watch(darkMode, applyTheme);

// lifecycle
onMounted(() => {
  // aplicar tema salvo (ou detectar do sistema)
  const savedTheme = localStorage.getItem("theme");
  if (savedTheme === "dark") {
    darkMode.value = true;
    applyTheme();
  } else if (savedTheme === "light") {
    darkMode.value = false;
    applyTheme();
  } else {
    // sem preferência salva: detecta do sistema
    detectThemeFromSystem();
  }

  // escuta mudança do sistema
  try {
    window
      .matchMedia("(prefers-color-scheme: dark)")
      .addEventListener("change", detectThemeFromSystem);
  } catch (e) {
    // navegadores antigos usam addListener
    const mm = window.matchMedia("(prefers-color-scheme: dark)");
    if (mm && mm.addListener) mm.addListener(detectThemeFromSystem);
  }

  // idioma salvo
  const savedLanguage = localStorage.getItem("language");
  if (savedLanguage) {
    locale.value = savedLanguage;
  }
});
</script>

<style scoped>
.page-root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: var(--bs-body-bg);
  color: var(--bs-body-color);
}

/* topbar */
.topbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 18px;
  border-bottom: 1px solid var(--bs-border-color);
}

.site-title {
  margin: 0;
  font-size: 1.4rem;
}

/* right side controls */
.topbar-right {
  display: flex;
  gap: 12px;
  align-items: center;
}

.lang-selector {
  position: relative;
}

.btn-small {
  padding: 6px 10px;
  border: 1px solid var(--bs-border-color);
  background: transparent;
  cursor: pointer;
}

.lang-list {
  position: absolute;
  right: 0;
  margin-top: 6px;
  background: var(--bs-body-bg);
  border: 1px solid var(--bs-border-color);
  list-style: none;
  padding: 6px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.06);
}

.lang-item {
  padding: 6px 10px;
  width: 100%;
  text-align: left;
  background: transparent;
  border: none;
  cursor: pointer;
}

/* main content */
.cursos-container {
  max-width: 900px;
  margin: 20px auto;
  padding: 18px;
  line-height: 1.6;
  font-family: Arial, sans-serif;
}

.curso {
  margin-bottom: 28px;
}

.curso-texto p {
  margin: 10px 0;
}
</style>