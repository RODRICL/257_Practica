<script setup lang="ts">
/* eslint-disable @typescript-eslint/no-explicit-any */
import type { Categoria } from '@/models/categoria'
import http from '@/plugins/axios'
import Button from 'primevue/button'
import Dialog from 'primevue/dialog'
import InputText from 'primevue/inputtext'
import { computed, ref, watch } from 'vue'

const ENDPOINT = 'categorias'
const props = defineProps({
  mostrar: Boolean,
  categoria: {
    type: Object as () => Categoria,
    default: () => ({}) as Categoria,
  },
  modoEdicion: Boolean,
})
const emit = defineEmits(['guardar', 'close'])

const dialogVisible = computed({
  get: () => props.mostrar,
  set: (value) => {
    if (!value) emit('close')
  },
})

const categoria = ref<Categoria>({ ...props.categoria })
watch(
  () => props.categoria,
  (newVal) => {
    categoria.value = { ...newVal }
  },
)

async function handleSave() {
  try {
    const body = {
      nombre: categoria.value.nombre,
      descripcion: categoria.value.descripcion,
    }
    if (props.modoEdicion) {
      await http.patch(`${ENDPOINT}/${categoria.value.id}`, body)
    } else {
      await http.post(ENDPOINT, body)
    }
    emit('guardar')
    categoria.value = {} as Categoria
    dialogVisible.value = false
  } catch (error: any) {
    alert(error?.response?.data?.message)
  }
}
</script>

<template>
  <div class="card flex justify-center">
    <Dialog
      v-model:visible="dialogVisible"
      :header="props.modoEdicion ? 'Editar' : 'Crear'"
      style="width: 25rem"
    >
      <div class="flex items-center gap-4 mb-4">
        <label for="nombre" class="font-semibold w-4">Nombre</label>
        <InputText
          id="nombre"
          v-model="categoria.nombre"
          class="flex-auto"
          autocomplete="off"
          autofocus
        />
      </div>
      <div class="flex items-center gap-4 mb-4">
        <label for="descripcion" class="font-semibold w-4">Descripción</label>
        <InputText
          id="descripcion"
          v-model="categoria.descripcion"
          class="flex-auto"
          autocomplete="off"
          autofocus
        />
      </div>  
      <div class="flex justify-end gap-2">
        <Button
          type="button"
          label="Cancelar"
          icon="pi pi-times"
          severity="secondary"
          @click="dialogVisible = false"
        ></Button>
        <Button type="button" label="Guardar" icon="pi pi-save" @click="handleSave"></Button>
      </div>
    </Dialog>
  </div>
</template>

<style scoped>
/* Estilo para los inputs (campos de formulario) */
.custom-input {
  background-color: rgb(0, 0, 0) !important; /* Fondo amarillo para los campos */
  border: 1px solid #ccc; /* Borde gris para los campos */
  color: rgb(255, 252, 252);  /* Color del texto en los inputs */
  padding: 0.5rem;
}

.custom-input:focus {
  border-color: #b3ff00;  /* Color de borde azul cuando el campo está en foco */
  outline: none;  /* Quitar el borde de enfoque predeterminado */
}
</style>
