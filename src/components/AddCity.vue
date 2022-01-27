<template>
    <form @submit.prevent="postData" @keydown="errors.clear($event.target.name)">
        <div class="modal is-active">
            <div class="modal-background" @click="$emit('close')"></div>

            <div class="modal-content">
                <div class="box">
                    <!-- <article class="message is-danger" v-if="errors !== null">
                        <div class="message-header">
                            <p>Klaida</p>
                            <button class="delete" aria-label="delete"></button>
                        </div>
                        <div class="message-body">
                            Nepavyko issiusti duomenu. Patikrinkite ivestus
                            duomenis
                        </div>
                    </article> -->

                    <!-- <article class="message is-success" v-if="okay">
                        <div class="message-header">
                            <p>Pavyko</p>
                        </div>
                        <div class="message-body">
                            Duomenys issiusti.
                        </div>
                    </article> -->

                    <p class="title">Prideti miesta</p>
                    <!-- Your content -->
                    <div class="field">
                        <input name="data.attributes.name" v-model="data.attributes.name" class="input" type="text" placeholder="Pavadinimas">
                        <span class="help is-danger" v-text="errors.get('data.attributes.name')"></span>
                    </div>
                    <div class="field">
                        <input name="data.attributes.area" v-model="data.attributes.area" class="input" type="number" placeholder="Uzimamas plotas">
                        <span class="help is-danger" v-text="errors.get('data.attributes.area')"></span>
                    </div>
                    <div class="field">
                        <input name="data.attributes.population" v-model="data.attributes.population" class="input" type="number" placeholder="Gyventoju skaicius">
                        <span class="help is-danger" v-text="errors.get('data.attributes.population')"></span>
                    </div>
                    <div class="field">
                        <input name="data.attributes.postal_code" v-model="data.attributes.postal_code" class="input" type="text" placeholder="Miesto pasto kodas">
                        <span class="help is-danger" v-text="errors.get('data.attributes.postal_code')"></span>
                    </div>
                    
                    <button type="submit" :disabled="errors.any()" class="button">Submit</button>

                    <span v-text="this.postResponse" class="help is-success"></span>
                    

                    <!-- <p class="help is-success">Success</p>
                    <p class="help is-danger">Error</p> -->
                </div>
            </div>

            <button class="modal-close is-large" aria-label="close" @click="$emit('close')"></button>
        </div>
    </form>

</template>

<script>

class Errors {
    constructor() {
        this.errors = {};
    }
    get(field) {
        if (this.errors[field]) {
            return this.errors[field][0];
        }
    }

    record(errors) {
        this.errors = errors
    }

    clear(field){
        // Vue.delete(this.errors, field)
        delete this.errors[field];
    }

    any(){

    }
}

// class Response {
//     constructor() {
//         this.postResponse = {};
//     }

//     record(postResponse) {
//         this.postResponse = postResponse
//     }

//     get(field) {
//         if (this.postResponse[field]) {
//             return this.postResponse[field][0];
//         }
//     }
// }

export default {
    name: 'AddCity',

    props: [
        'showModal',
        // 'response'
    ],

    data() {
        return {
            
            errors: new Errors(),
            id: this.$route.params.id,
            // okay: false,
            data: {
                attributes: {
                    name: '',
                    area: '',
                    population: '',
                    postal_code: '',
                }
            },
            // postResponse: new Response()
            postResponse: ''

        }
    },

    methods: {
        postData() {
            return this.$http.post('https://akademija.teltonika.lt/countries_api/api/countries/' + this.id + "/cities", {
                // name: this.name,
                // area: this.area,
                // population: this.population,
                // postal_code: this.postal_code
                data: this.data
            })
            .then(this.onPostedSuccess)
            .catch(error => this.errors.record(error.response.data.errors))
        },

        onPostedSuccess(response) {
            this.postResponse = response.data.message;

            this.data.attributes.name = '';
            this.data.attributes.area = '';
            this.data.attributes.population = '';
            this.data.attributes.postal_code = '';
        }
    }

}
</script>

<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>
