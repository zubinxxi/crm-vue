<script setup>
    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';


    const props = defineProps({
        cliente:{
            type:Object
        }
    })

    const emit = defineEmits(['actualizar-estado', 'eliminar-cliente'])

    const nombreCliente = computed(()=>{
        return props.cliente.nombre + ' ' + props.cliente.apellido
    })

    const estadoCliente = computed(()=>{
        return props.cliente.estado
    })

</script>

<template>
    <tr>
        <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm sm:pl-0">
            <p class="font-medium text-gray-900">{{ nombreCliente }}</p>
            <p class="text-gray-500">{{ cliente.email }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
            <p class="text-gray-900 font-bold">{{ cliente.empresa }}</p>
            <p class="text-gray-600">{{ cliente.puesto }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm">
            <button
                class="inline-flex rounded-full px-2 text-xs font-semibold leading-5"
                :class="[cliente.estado ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800']"
                @click="$emit('actualizar-estado', { id: cliente.id, estado: cliente.estado })"
            >
                {{ cliente.estado ? 'Activo' : 'Inactivo' }}
            </button>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 ">
            <RouterLink 
                class="text-indigo-600 hover:text-indigo-900 mr-5"
                :to="{ name: 'editar-cliente', params: { id: cliente.id } }"
            >
                Editar
            </RouterLink>

            <button 
                href="#"
                class="text-red-600 text-red-900"
                @click="$emit('eliminar-cliente', cliente.id)"
            >
                Borrar
            </button>
        </td>
    </tr>
</template>