<template>
    <div class="ion-page">
        <div>
            <ion-header>
                <ion-toolbar>
                    <ion-title>Départements</ion-title>
                </ion-toolbar>
            </ion-header>
            <ion-item>
                <ion-label position="floating">Code département</ion-label>
                <ion-input :value="zipcode" @input="zipcode = $event.target.value"></ion-input>
            </ion-item>
            <div class="ion-padding">
                <ion-button @click.prevent="sendForm()" expand="block" type="submit"
                            class="ion-no-margin ios button button-block button-solid ion-activatable ion-focusable hydrated activated">
                    Send
                </ion-button>
            </div>
        </div>
        <ion-content>
            <ion-card class="ion-padding" v-for="item in array">
                <ion-card-title>{{ item.nom }}</ion-card-title>
                <ion-card-subtitle>Code postal : {{ item.codesPostaux[0] }}</ion-card-subtitle>
                <ion-card-subtitle>Code département : {{ item.codeDepartement }}</ion-card-subtitle>
                <ion-card-subtitle>Population : {{ item.population }}</ion-card-subtitle>
            </ion-card>
        </ion-content>
        <container/>

    </div>
</template>

<script>
    import Container from '../components/Container.vue'

    export default {
        name: "home",
        components: {
            Container
        },
        data() {
            return {
                zipcode: '',
                array: []
            }
        },
        methods: {
            sendForm() {
                let reg = /[0-9]{2}/
                if (reg.test(this.zipcode)) {
                    this.$http.get('https://geo.api.gouv.fr/departements/' + this.zipcode + '/communes').then(res => {
                        this.array = res.data
                    })
                }else {
                    this.presentAlert('Le code postal ou la ville est incorrecte')
                }
                this.zipcode = null
            },
            presentAlert(message) {
                return this.$ionic.alertController
                    .create({
                        header: 'Erreur',
                        message: message,
                        buttons: ['OK'],
                    }).then(a => a.present())
            }
        }
    }
</script>
