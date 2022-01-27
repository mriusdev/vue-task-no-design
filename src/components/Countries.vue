<template>
    <div class="container">
        <div class="field">
            <h3 class="title">Country search</h3>
            <button class="button" @click="showModal = true">Add country</button>
            <input class="input" v-model="search" type="text" placeholder="Search for country">
            <button class="button is-link" @click.prevent="getData">Search</button>
        </div>
        
        <button class="button is-info is-light" @click="sortedArray">Sort array by descending</button>
        <!-- <p v-if="!this.descending" class="help is-success">Descending V</p>
        <p v-if="!this.descending" class="help is-success">Ascending ^</p> -->

        <AddCountry v-if="showModal" @close="showModal = false"></AddCountry>
        <!-- <EditCountry v-if="showEdit" @close="showEdit = false"></EditCountry> -->

        <!-- <div class="box" v-for="country in countries.data" :key="country.id"> -->
        <div class="box" v-for="country in countries" :key="country.id">

            <router-link :to="'/countries/' + country.id + '/cities'">
                <h3 class="title">{{ country.attributes.name }}</h3>
            </router-link>
            
            <p class="body">{{ country.attributes.population }}</p>

            <div class="field">
              <router-link :to="'/countries/' + country.id + '/edit'">
                
                  <button class="button is-info">Edit</button>
                
              </router-link>
            </div>
            

            <div class="field">
              <button @click="deleteCountry(country)" class="button is-danger">Delete</button>
            </div>
            
        </div>

        <nav v-if="meta.links && meta.links.length" class="pagination is-centered" role="navigation" aria-label="pagination">

            <router-link class="pagination-previous" v-if="meta.current_page !== 1" :to="{ query: { page: meta.current_page - 1 }}">
              Previous
            </router-link>
            

            <!-- <router-link :to="{ query: { page: meta.current_page - 1 }}" class="pagination-previous" :class="{'is-disabled' : meta.current_page === 1 }">
              Previous
            </router-link> -->

            <router-link v-if="meta.current_page !== meta.last_page" class="pagination-next" :to="{ query : { page: meta.current_page + 1 }}">
              Next page
            </router-link>

          <ul class="pagination-list">
            <li v-for="page in meta.last_page" :key="page" >
                <router-link :class="{'is-current' : page === meta.current_page}" class="pagination-link" :to="{ query : { page: page }}">
                  {{ page }}
                </router-link>
            </li>

            <!-- <li><a class="pagination-link is-current" aria-label="Page 46" aria-current="page">46</a></li>
            <li><a class="pagination-link" aria-label="Goto page 86">86</a></li> -->
          </ul>

        </nav>
    </div>
</template>

<script>
// import searchMixin from '../mixins/searchMixin'
import AddCountry from './AddCountry'
// import EditCountry from './EditCountry'

export default {
    name: 'Countries',
    components: {
      AddCountry,
      // EditCountry
    },
    
    data() {
        return {
            meta: {},
            countries: [],
            search: '',
            showModal: false,
            descending: null
            // showEdit: false
            // response: true,
        };
    },

    watch: {
      '$route.query.page': {
        immediate: true,
        handler(page) {
          page = parseInt(page) || 1;
          if ( page !== this.meta.current_page ) {
            this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries?page=${page}`)
            .then(response => {
              this.meta = response.data.meta;
              this.countries = response.data.data;
            })
          }
        }
      }
    },

    methods: {
      getData(page) {
        console.log(`Checking term ${this.search}`);
        this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries?page=${page}&search=${this.search}`)
        .then(response => {
          console.log(response.data.results);
          this.countries = response.data.data
        })
        .catch(errors => {
          console.log(errors);
        })

      },

      deleteCountry(country){
        let choice = confirm(`Are you sure you want to delete ${country.attributes.name}?`)
        
        if(choice) {
          let testCountries = this.countries.filter(cntry => cntry.id != country.id)
          this.countries = testCountries;
          
          this.$http({
          method: 'delete',
          url: 'https://akademija.teltonika.lt/countries_api/api/countries/' + country.id
          })
          .then(response =>{
            console.log(response.data.message);
          })
          .catch(errors => {
            console.log(errors);
          })
        }
        
      },

      sortedArray(){
        if(this.descending == false){
          this.descending = true;
          return this.countries.sort((a, b) => b.attributes.population - a.attributes.population);
      
        } else {
          this.descending = false;
          return this.countries.sort((a, b) => a.attributes.population - b.attributes.population);
        }
        
        
      }
    },

      

  // mixins: [searchMixin]
  
};
</script>