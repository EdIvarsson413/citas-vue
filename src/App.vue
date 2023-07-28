<template>
  <div class="container mx-auto mt-8">
    <Header />

    <!-- Contenido del sitio -->
    <div class="mt-12 md:flex lg:px-3">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de 
            <span class="text-transparent bg-clip-text font-bold
                bg-gradient-to-bl from-[#41B883] to-[#35495E]">
                Pacientes
            </span>
          </p>

          <Paciente
            v-for="paciente in pacientes"
            :key="paciente.id"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch, onMounted } from 'vue'
import { uid } from 'uid'
import Header from '@/Header.vue'
import Formulario from '@/Formulario.vue'
import Paciente from '@/Paciente.vue'

const pacientes = ref([]);

// Reactive es una mejor opcion para no tener muchas variables con ref
const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: 'No come'
})

// Cuando se incie la app carga los pacientes de LS
onMounted(() => {
  const pacientesLS = localStorage.getItem( "pacientes" );
  if( pacientesLS ) pacientes.value = JSON.parse( pacientesLS );
})

// Observador de cambios
watch( pacientes , () => {
  localStorage.setItem( "pacientes", JSON.stringify( pacientes.value ) );
}, {
  deep: true
})

// Guardar paciente en el objeto
const guardarPaciente = () => {
  // Si esta ocurriendo una edicion de paciente
  if( paciente.id ) {
    // Reemplaza el elemento 
    const { id } = paciente;

    // Busca el indice del elemento por medio del id actual
    const index = pacientes.value.findIndex( paciente => paciente.id === id );

    // Reemplaza y mantiene los datos originales
    pacientes.value[index] = {...paciente}
  } else {
    // Añade paciente
    pacientes.value.push({ ...paciente, id: uid() });
  }
  
  // Reiniciar el formulario
  Object.assign( paciente, {
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
  })
  
}

// Editar paciente
const actualizarPaciente = (id) => {
  // Buscando el paciente con su id
  const pacienteEditar = pacientes.value.find( paciente => paciente.id === id );
  
  // Los datos pasan al objeto y se muestran en el formulario
  Object.assign( paciente, pacienteEditar );
}

// Eliminar un paciente
const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter( paciente => paciente.id !== id);
}
</script>