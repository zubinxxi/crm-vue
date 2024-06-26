<script setup>
    import ClienteService from '../services/ClienteService'
    import RouterLinkBlue from '../components/UI/RouterLinkBlue.vue'
    import Heading from '../components/UI/Heading.vue'
    import { FormKit } from '@formkit/vue'
    import { useRouter } from 'vue-router'


   
    const router = useRouter()

    defineProps({
        titulo: {
            type: String
        }
    })


    /*const formData = {
        nombre: 'Zubin',
        apellido: 'Juárez',
        email: 'zubinxxi@gmail.com',
        telefono: '6252-6378',
        empresa: 'zubinweb.com',
        puesto: 'CEO'
    }*/

    const handleSubmit = (data)=>{
        data.estado = 1
        ClienteService.agregarClientes(data)
        .then(respuesta => {
            //Redireccionar
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
                    submit-label="Agregar Cliente"
                    incomplete-message="No se pudo enviar, revisa los mensajes" 
                    @submit="handleSubmit"
                >
                    <FormKit 
                        type="text"
                        label="Nombre"
                        name="nombre"
                        placeholder="Nombre del cliente"
                        validation="required"
                        :validation-messages="{required:'El nombre del cliente es obligatorio'}"
                        
                    />

                    <FormKit 
                        type="text"
                        label="Apellido"
                        name="apellido"
                        placeholder="Apellido del cliente"
                        validation="required"
                        :validation-messages="{required:'El apellido del cliente es obligatorio'}" 
                    />

                    <FormKit 
                        type="email"
                        label="Email"
                        name="email"
                        placeholder="Email del cliente"
                        validation="required|email"
                        :validation-messages="{required:'El correo del cliente es obligatorio', email:'Coloca un email válido'}" 
                        prefix-icon="email"
                    />

                    <FormKit 
                        type="text"
                        label="Teléfono"
                        name="telefono"
                        placeholder="Teléfono: XXXX-XXXX"
                        validation="*matches:/^[0-9]{4}-[0-9]{4}$/"
                        :validation-messages="{matches:'El formato no es válido'}" 
                        prefix-icon="telephone"
                    />

                    <FormKit 
                        type="text"
                        label="Empresa"
                        name="empresa"
                        placeholder="Empresa del cliente"
                    />

                    <FormKit 
                        type="text"
                        label="Puesto"
                        name="puesto"
                        placeholder="Puesto del cliente"
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