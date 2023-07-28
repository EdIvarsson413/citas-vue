<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-transparent bg-clip-text font-bold
                    bg-gradient-to-bl from-[#41B883] to-[#35495E]">
                Administralos
            </span>
        </p>

        <!-- Mensaje automatico -->
        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <!-- Formulario -->
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" autocomplete="off" @submit.prevent="validar">
            <!-- Nombre de la mascota -->
            <div class="mb-5">
                <label 
                    for="mascota"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre de mascota
                </label>
                <input 
                    id="mascota" 
                    type="text"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder:text-gray-400 rounded-md
                            focus:ring-2 focus:ring-emerald-500 focus:outline-none"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                >
            </div>
            
            <!-- Nombre del propietario -->
            <div class="mb-5">
                <label 
                    for="propietario"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre del propietario
                </label>
                <input 
                    id="propietario" 
                    type="text"
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder:text-gray-400 rounded-md
                            focus:ring-2 focus:ring-emerald-500 focus:outline-none"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                >
            </div>

            <!-- Correo -->
            <div class="mb-5">
                <label 
                    for="correo"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                    id="correo" 
                    type="email"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder:text-gray-400 rounded-md
                            focus:ring-2 focus:ring-emerald-500 focus:outline-none"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                >
            </div>

            <!-- Alta -->
            <div class="mb-5">
                <label 
                    for="alta"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                <input 
                    id="alta" 
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder:text-gray-400 rounded-md
                            focus:ring-2 focus:ring-emerald-500 focus:outline-none"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                >
            </div>

            <!-- Sintomas -->
            <div class="mb-5">
                <label 
                    for="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea
                    id="alta"
                    placeholder="Describa que es lo que tiene su mascota"
                    class="border-2 w-full p-2 mt-2 placeholder:text-gray-400 rounded-md
                            focus:ring-2 focus:ring-emerald-500 focus:outline-none h-22"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                ></textarea>
            </div>
            
            <!-- Boton -->
            <input 
                type="submit"
                class="w-full p-3 text-white font-bold uppercase cursor-pointer rounded-md
                    hover:bg-gradient-to-br transition-colors
                    bg-gradient-to-bl from-[#41B883] to-[#35495E]"
                :value="[editando ? 'Guardar Cambios' : 'Registrar paciente']"
            >
        </form>
    </div>
</template>

<script setup>
import { reactive, computed } from 'vue'
import Alerta from '@/Alerta.vue'

// Se definen emits para que los vModel puedan ser escuchados y el formulario registre los datos
// Si se define un emit se pasa al componente que lo usorá
const emit = defineEmits([
    'update:nombre',
    'update:propietario',
    'update:email',
    'update:alta',
    'update:sintomas',
    'guardar-paciente'
])

// Tambien se deben de declarar los props para utilizar las variables de los input
const props = defineProps({
    id: {
        type: [String, null],
        required: true
    },
    nombre: {
        type: String,
        required: true,
    },
    propietario: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
    },
    alta: {
        type: String,
        required: true,
    },
    sintomas: {
        type: String,
        required: true,
    },
})

// State para agregar una alerta con un mensaje dinamico
const alerta = reactive({
    tipo: '',
    mensaje: ''
})

// La propiedad determina la etiqueta del boton
const editando = computed(() => {
    return props.id
})

const validar = () => {
    // Los datos del paciente ya se encuentran en props, solo se pasan a la validación
    if( Object.values( props ).includes('') ){
        alerta.tipo = 'error';
        alerta.mensaje = 'Todos los campos son obligatorios';

        // Se limpia la alerta
        setTimeout(() => {
            alerta.mensaje = ''
        }, 3000)
        return
    }
    
    // defineEmits pasa a una varibale para usar los emits dentro del script
    emit('guardar-paciente')

    alerta.tipo = 'exito';
    alerta.mensaje = 'Datos agregados correctamente';

    // Se limpia la alerta
    setTimeout(() => {
        alerta.mensaje = ''
    }, 3000)
}

</script>