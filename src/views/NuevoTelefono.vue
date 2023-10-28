<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>
                    Inventario
                </ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="ID"
                            :disabled="true"
                            v-model="id"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="Nombre del telefono"
                            v-model="phone.name"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="Año"
                            type="number"
                            v-model="phone.data.year"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="Precio"
                            type="number"
                            v-model="phone.data.price"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="CPU model"
                            v-model="phone.data['CPU model']"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="12">
                        <ion-input
                            label-placement="stacked"
                            fill="outline"
                            shape="round"
                            color="success"
                            label="Capacidad de memoria"
                            v-model="phone.data['Hard disk size']"
                        >

                        </ion-input>
                    </ion-col>
                    <ion-col size="4">
                        <ion-button
                            shape="round"
                            expand="full"
                            color="primary"
                            @click="agregarTelefono"
                        >
                            Agregar
                        </ion-button>
                    </ion-col>
                    <ion-col size="4">
                        <ion-button
                            shape="round"
                            expand="full"
                            color="success"
                            @click="editarTelefono"
                        >
                            Editar
                        </ion-button>
                    </ion-col>
                    <ion-col size="4">
                        <ion-button
                            shape="round"
                            expand="full"
                            color="danger"
                            @click="eliminarTelefono"
                        >
                            Eliminar
                        </ion-button>
                    </ion-col>
                </ion-row>
            </ion-grid>
            <!-- Toast para mensajes -->
            <ion-toast :icon="informationOutline"
                :message="toastMessage"
                :duration="2000"
                :isOpen="errorState"
                @didDismiss="showToast(false)"
            >

            </ion-toast>
        </ion-content>
    </ion-page>
        
    
</template>


<script>
import {informationOutline} from 'ionicons/icons' 
import axios from 'axios';
import {IonPage, IonHeader, IonToolbar, IonTitle, 
        IonContent, IonGrid, IonRow, IonCol, 
        IonInput, IonButton, IonToast} from '@ionic/vue'
export default{
    name: 'NuevoTelefono',
    components: {
        IonPage, IonHeader, IonToolbar, IonTitle, 
        IonContent, IonGrid, IonRow, IonCol, 
        IonInput, IonButton, IonToast
    }, 
    data(){
        return{
            // para editar y eliminar telefono
            id: 0, 
            // para guardar datos del form
            phone: {
                "name": "Iphone 15 pro max", 
                "data": {
                    "year": 2023, 
                    "price": 1500, 
                    "CPU model": "A15 pro", 
                    "Hard disk size": "1 TB"
                }
            },
            errorState: false, 
            toastMessage: '', 
            informationOutline, 

        }
    }, 
    methods: {
        agregarTelefono(){
            // consumir endpoint para insertar telefono
            axios.post("https://api.restful-api.dev/objects", this.phone)
                .then(response => {
                    console.log(response.data);
                    this.id = response.data.id; 
                    this.showToast(true, 'Teléfono agregado')

                })
                .catch(error => console.log(error))
        }, 
        editarTelefono(){
            if(this.id !== 0){
                // consumir endpoint para editar
                axios.put(`https://api.restful-api.dev/objects/${this.id}`, this.phone)
                .then(response => {
                    console.log(response.data);
                this.showToast(true, 'Teléfono actualizado')
                }) 
                .catch(error => console.log(error))
            }else{
                this.showToast(true, 'El ID de teléfono no existe');
            }
           
        },
        eliminarTelefono(){
            // consumir endpoint para eliminar
            if(this.id !== 0){
                axios.delete(`https://api.restful-api.dev/objects/${this.id}`)
                    .then(response => {
                        console.log(response.data);
                        this.showToast(true, 'Teléfono eliminado');
                        this.id = 0;
                    })
                    .catch(error => console.log(error))
            }else{
                this.showToast(true, 'El ID del teléfono no existe');
            }
        },
        showToast(state, message){
            this.errorState = state;
            this.toastMessage = message;
        }
    }
}


</script>


<style>

</style>