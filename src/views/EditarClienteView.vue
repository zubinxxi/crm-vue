<script setup>
    import { ref,onMounted } from 'vue'
    import ClienteService from '../services/ClienteService'
    import RouterLinkBlue from '../components/UI/RouterLinkBlue.vue'
    import Heading from '../components/UI/Heading.vue'
    import { FormKit } from '@formkit/vue'
    import { useRouter, useRoute } from 'vue-router'
   
    const router = useRouter()
    const route = useRoute()

    const {id} =route.params
    
    const formDataCliente = ref({})
    
    onMounted(()=>{
        ClienteService.obtenerCliente(id)
        .then(respuesta => {
            //
            formDataCliente.value = respuesta.data
        })
        .catch(error => console.log(error))
    })

    defineProps({
        titulo: {
            type: String
        }
    })

    const handleSubmit = (data)=>{
       ClienteService.actualizarCliente(id, data)
        .then(()=>{
            router.push({name: 'inicio'})
        })
        .catch(error => console.log(error))
    }

</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLinkBlue to="inicio">
                Volver
            </RouterLinkBlue>
        </div>

        <Heading>{{ titulo }}</Heading>

        <div class="mx-auto mt-10 bg-white shadow">
            <div class="mx-auto md:w-1/2 py-20 px-6">
                <FormKit
                    type="form"
                    submit-label="Guardar Cambios"
                    incomplete-message="No se pudo enviar, revisa los mensajes" 
                    @submit="handleSubmit"
                    :value="formDataCliente"
                >
                    <FormKit 
                        type="text"
                        label="Nombre"
                        name="nombre"
                        placeholder="Nombre del cliente"
                        validation="required"
                        :validation-messages="{required:'El nombre del cliente es obligatorio'}"
                        v-model="formDataCliente.nombre"
                        
                    />

                    <FormKit 
                        type="text"
                        label="Apellido"
                        name="apellido"
                        placeholder="Apellido del cliente"
                        validation="required"
                        :validation-messages="{required:'El apellido del cliente es obligatorio'}" 
                        v-model="formDataCliente.apellido"
                    />

                    <FormKit 
                        type="email"
                        label="Email"
                        name="email"
                        placeholder="Email del cliente"
                        validation="required|email"
                        :validation-messages="{required:'El correo del cliente es obligatorio', email:'Coloca un email válido'}" 
                        prefix-icon="email"
                        v-model="formDataCliente.email"
                    />

                    <FormKit 
                        type="text"
                        label="Teléfono"
                        name="telefono"
                        placeholder="Teléfono: XXXX-XXXX"
                        validation="*matches:/^[0-9]{4}-[0-9]{4}$/"
                        :validation-messages="{matches:'El formato no es válido'}" 
                        prefix-icon="telephone"
                        v-model="formDataCliente.telefono"
                    />

                    <FormKit 
                        type="text"
                        label="Empresa"
                        name="empresa"
                        placeholder="Empresa del cliente"
                        v-model="formDataCliente.empresa"
                    />

                    <FormKit 
                        type="text"
                        label="Puesto"
                        name="puesto"
                        placeholder="Puesto del cliente"
                        v-model="formDataCliente.puesto"
                    />

                    
                </FormKit>
            </div>
            
        </div>
    </div>
</template>

<style>
    .formkit-wrapper {
        max-width: 100% !important; 
    }
</style>