<template>
    <div class="container">
        
        <div class="box" v-if="city.data.attributes != null">

            <div class="control">
                <p class="subtitle is-5">Name</p>
                <textarea v-model="city.data.attributes.name" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Area</p>
                <textarea v-model="city.data.attributes.area" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Population</p>
                <textarea v-model="city.data.attributes.population" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Phone code</p>
                <textarea v-model="city.data.attributes.postal_code" class="textarea has-fixed-size"></textarea>
            </div>
            
            <div class="field">
                <button @click="saveCity()" class="button is-success">Save changes</button>
            </div>

            <div class="field" v-if="this.responseMsg">
                <span v-text="this.responseMsg" class="help is-success"></span>
                <router-link :to="{ name: 'home' }">
                    <button class="button is-success is-light">Grizti i pagrindini langa</button>
                </router-link>
                
            </div>
            

            

        </div>
    </div>
</template>

<script>
export default {
    name: 'CityEdit',
    data(){
        return {
            city: {
                data: {}
            },
            id: this.$route.params.id,
            idCity: this.$route.params.idCity,
            responseMsg: null
        }
    },

    methods: {
        getData() {
            this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries/${this.id}/cities/${this.idCity}`)
            .then(response => {
                this.city.data = response.data.data;
                console.log(this.city);
            })
            .catch(errors => {
                console.log(errors);
            })
        },

        saveCity() {
            this.$http.put('https://akademija.teltonika.lt/countries_api/api/countries/' + this.id + '/cities/' + this.idCity, {
                data: this.city.data
            })
            .then(response =>{
                console.log(response.data.message);
                this.responseMsg = response.data.message;
                
            })
            .catch(errors => {
            console.log(errors);
            })

            //istrinam vids is musu local array
            // let updatedCountry = this.country.forEach( cntry => {
            //     if(cntry.id == country.id) {
            //         cntry = country
            //     }
            //     return cntry = updatedCountry
                
            // })

            
        }
        
    },

    mounted() {
        this.getData();
    },
}
</script>

<style>

</style>