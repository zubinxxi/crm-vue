<script setup>    
    import { ref, onMounted, computed } from 'vue';
    import ClienteService from '../services/ClienteService'
    import RouterLinkBlue from '../components/UI/RouterLinkBlue.vue'
    import Heading from '../components/UI/Heading.vue'
    import Cliente from '../components/Cliente.vue'
    

    const clientes = ref([])

    onMounted(()=>{
        ClienteService.obtenerClientes()
        //const url ='http://localhost:4000/clientes'
        //axios.get(url)
        //promesa con distruction
            .then(({data}) =>{
                clientes.value = data
            })
            //promesa tradicional
            /*.then(respuesta =>{
                console.log(respuesta.data)
            })*/
            .catch(error => {
                console.log('Hubo un error')
            })
    })

    const existenClientes = computed(()=>{
        return clientes.value.length > 0
    })

    defineProps({
        titulo: {
            type: String
        }
    })

    const actualizarEstado = (data) =>{
        ClienteService.cambiarEstado(data.id, {estado: !data.estado})
            .then(()=>{
                const i = clientes.value.findIndex(cliente => cliente.id === data.id)
                clientes.value[i].estado = !data.estado
                //router.push({name: 'inicio'})
            })
            .catch(error => console.log(error))
    }

    const eliminarCliente = (id) =>{
        ClienteService.eliminarCliente(id)
            .then(()=>{
                clientes.value = clientes.value.filter(cliente => cliente.id !== id)
            })
            .catch(error => console.log(error))
    }
</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLinkBlue to="agregar-cliente">
                Agregar Cliente
            </RouterLinkBlue>
        </div>
        <Heading>{{ titulo }}</Heading>
        <div v-if="existenClientes" class="flow-root mx-auto  mt-10 p-5 bg-white shadow">
            <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
                <div class="min-w-full py-2 align-middle sm:px-6 lg:px-8">
                    <table class="min-w-full divide-y divide-gray-300">
                        <thead>
                        <tr>
                            <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Nombre</th>
                            <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Empresa</th>
                            <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Estado</th>
                            <th scope="col" class="p-2 text-left text-sm font-extrabold text-gray-600">Acciones</th>
                        </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-200 bg-white">
                            <!--<tr v-for="cliente in clientes" :key="cliente.id">
                                <td>{{ cliente.nombre }}</td>
                                <td>{{ cliente.empresa }}</td>
                                <td>Activo</td>
                                <td>editar|Borrar</td>

                            </tr>-->
                            <Cliente 
                                v-for="cliente in clientes" 
                                :key="cliente.id"
                                :cliente="cliente"
                                @actualizar-estado="actualizarEstado"
                                @eliminar-cliente="eliminarCliente"
                            />

                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <p v-else class="text-center mt-10">No hay clientes</p>
    </div>
</template>

<style scoped>

</style>