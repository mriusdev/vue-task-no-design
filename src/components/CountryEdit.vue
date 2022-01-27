<template>
    <div class="container">
        <!-- <div class="box" v-for="country in countries" :key="country.id"> -->
        <div class="box" v-if="country.data.attributes != null">
            <!-- <h3 class="title">{{ country.attributes.name }}</h3>
            <p class="body">{{ country.attributes.area }}</p>
            <p class="body">{{ country.attributes.population }}</p>
            <p class="body">{{ country.attributes.phone_code }}</p> -->

            <div class="control">
                <p class="subtitle is-5">Name</p>
                <textarea v-model="country.data.attributes.name" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Area</p>
                <textarea v-model="country.data.attributes.area" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Population</p>
                <textarea v-model="country.data.attributes.population" class="textarea has-fixed-size"></textarea>
            </div>

            <div class="control">
                <p class="subtitle is-5">Phone code</p>
                <textarea v-model="country.data.attributes.phone_code" class="textarea has-fixed-size"></textarea>
            </div>
            
            <div class="field">
                <button @click="saveCountry()" class="button is-success">Save changes</button>
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
    name: 'CountryEdit',
    data(){
        return {
            country: {
                data: {}
            },
            id: this.$route.params.id,
            responseMsg: null
        }
    },

    methods: {
        getData() {
            this.$http.get("https://akademija.teltonika.lt/countries_api/api/countries/" + this.id)
            .then(response => {
                this.country.data = response.data.data;
                console.log(this.country);
            })
            .catch(errors => {
                console.log(errors);
            })
        },

        saveCountry() {
            // this.$http({
            //     method: 'put',
            //     url: 'https://akademija.teltonika.lt/countries_api/api/countries/' + this.id
            // })
            this.$http.put('https://akademija.teltonika.lt/countries_api/api/countries/' + this.id, {
                data: this.country.data
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