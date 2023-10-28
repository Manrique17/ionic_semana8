<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>
                    Teléfonos
                </ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
            <ion-list lines="full" v-for="(phone, i) in phones" :key="i">
                <ion-item>
                    <ion-icon :icon="checkmarkCircle" color="success" slot="start">

                    </ion-icon>
                    <ion-label>
                        {{ phone.name }}
                        <spam v-if="phone.data && phone.data.hasOwnProperty('color')">
                        ({{ phone.data.color }}) 
                        </spam>
                    </ion-label>
                    
                </ion-item>
            </ion-list>
        </ion-content>
    </ion-page>
</template>


<script>
import axios from 'axios';
import {IonPage, IonHeader, IonToolbar, IonTitle, 
        IonContent, IonList, IonItem, IonLabel, IonIcon} from '@ionic/vue'
import {checkmarkCircle} from 'ionicons/icons'
 export default{
    name: 'TelefonoList', 
    components: {
        IonPage, IonHeader, IonToolbar, IonTitle, 
        IonContent, IonList, IonItem, IonLabel, IonIcon
    }, 
    data(){
        return{
            checkmarkCircle, 
            phones: []
        }
    }, 
    methods: {
        // Va a consumir endpoint y extraera telefonos registrados
        getTelefonos(){
            axios.get('https://api.restful-api.dev/objects')
                .then(response => {
                    // Agregar datos al array phones
                    this.phones = response.data; 
                    console.log(response);

                })
                .catch(error => {
                    console.log(error)
                }) 
        }
    }, 
    mounted(){
        this.getTelefonos();
    }
 }
</script>


<style>

</style>