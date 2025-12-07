<template>
  <div class="ag-format-container">
    <div class="ag-courses_box">
      <div class="ag-courses_item" 
           v-for="curso in cursos" 
           :key="curso.tituloKey">
        
        <a href="#" class="ag-courses-item_link" 
           @click.prevent="abrirDescricao(curso)">
          
          <div class="ag-courses-item_bg" :style="{ backgroundColor: curso.cor }"></div>

          <div class="ag-courses-item_title">
            {{ t(curso.tituloKey) }}
          </div>

          <div class="ag-courses-item_type">
            {{ t(curso.tipoKey) }}
          </div>

        </a>
      </div>
    </div>
  </div>

  <!-- ⭐ MODAL DA DESCRIÇÃO DO CURSO -->
  <div v-if="cursoSelecionado" class="modal-overlay" @click="fecharDescricao">
    <div class="modal-content" @click.stop>
      <button class="close-btn" @click="fecharDescricao">X</button>

      <h2>{{ t(cursoSelecionado.tituloKey) }}</h2>
      <p>{{ t(cursoSelecionado.descriptionKey) }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useI18n } from "vue-i18n";

const { t } = useI18n();

interface Curso {
  tituloKey: string;
  tipoKey: string;
  descriptionKey: string;
  cor: string;
}

const cursoSelecionado = ref<Curso | null>(null);

function abrirDescricao(curso: Curso) {
  cursoSelecionado.value = curso;
}

function fecharDescricao() {
  cursoSelecionado.value = null;
}

const cursos = ref<Curso[]>([
  {
    tituloKey: "courses.civil_eng",
    tipoKey: "courses.bachelor",
    descriptionKey: "courses.civil_desc",
    cor: "#f9b234",
  },
  {
    tituloKey: "courses.comp_eng",
    tipoKey: "courses.bachelor",
    descriptionKey: "courses.comp_desc",
    cor: "#3ecd5e",
  },
  {
    tituloKey: "courses.biopro_eng",
    tipoKey: "courses.bachelor",
    descriptionKey: "courses.biopro_desc",
    cor: "#e44002",
  },
  {
    tituloKey: "courses.electro_eng",
    tipoKey: "courses.bachelor",
    descriptionKey: "courses.electro_desc",
    cor: "#952aff",
  },
  {
    tituloKey: "courses.math_lic",
    tipoKey: "courses.licentiate",
    descriptionKey: "courses.math_desc",
    cor: "#cd3e94",
  },
  {
    tituloKey: "courses.chem_proc_tec",
    tipoKey: "courses.technologist",
    descriptionKey: "courses.chem_desc",
    cor: "#4c4ea9",
  },
]);
</script>
<style>
@import "@/styles/Courses.css";
</style>
<style>
  .modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.6);
  backdrop-filter: blur(3px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background: #1e1e1e;
  padding: 25px;
  border-radius: 12px;
  width: 70%;
  max-width: 500px;
  color: white;
  position: relative;
  box-shadow: 0 0 20px rgba(0,0,0,0.5);
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 15px;
  background: #ff4c4c;
  border: none;
  border-radius: 6px;
  color: white;
  padding: 5px 10px;
  cursor: pointer;
}
</style>