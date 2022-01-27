<template>
  <div class="container">
    <div class="field">
      <h3 class="title">City search</h3>
      <button class="button" @click="showModal = true">Add city</button>
      <input
        class="input"
        v-model="search"
        type="text"
        placeholder="Search for city"
      />
      <button class="button is-link" @click.prevent="getData">Search</button>
    </div>

    <button class="button is-info is-light" @click="sortedArray">Sort array by descending</button>

    <AddCity v-if="showModal" @close="showModal = false"></AddCity>
    
    <div class="box" v-for="city in cities" :key="city.id">
      <div class="content is-medium">
        <h3 class="title">{{ city.attributes.name }}</h3>
        <article>{{ city.attributes.population }}</article>
      </div>

      <!-- <div class="field">
        <router-link :to="'/countries/' + this.id + '/cities/' + this.idCity + '/edit'">
          <button class="button is-info">Edit</button>
        </router-link>
      </div> -->

      <div class="field">
        <button @click="deleteCity(city)" class="button is-danger">
          Delete
        </button>
      </div>
    </div>

    <!-- <nav
      v-if="meta.links && meta.links.length"
      class="pagination is-centered"
      role="navigation"
      aria-label="pagination"
    >
      <router-link
        class="pagination-previous"
        v-if="meta.current_page !== 1"
        :to="{ query: { page: meta.current_page - 1 } }"
      >
        Previous
      </router-link>

      <router-link
        v-if="meta.current_page !== meta.last_page"
        class="pagination-next"
        :to="{ query: { page: meta.current_page + 1 } }"
      >
        Next page
      </router-link>

      <ul class="pagination-list">
        <li v-for="page in meta.last_page" :key="page">
          <router-link
            :class="{ 'is-current': page === meta.current_page }"
            class="pagination-link"
            :to="{ query: { page: page } }"
          >
            {{ page }}
          </router-link>
        </li>
      </ul>
    </nav> -->

  </div>
</template>

<script>
// import searchCity from '../mixins/searchCity'
import AddCity from "./AddCity";

export default {
  name: "SingleCountry",
  components: {
    AddCity,
  },

  data() {
    return {
        // meta: {},
      cities: [],
      id: this.$route.params.id,
      idCity: this.$route.params.idCity,
      search: "",
      showModal: false,
    };
  },

//   watch: {
//     "$route.query.page": {
//       immediate: true,
//       handler(page) {
//         page = parseInt(page) || 1;
//         if (page !== this.meta.current_page) {
//           this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries/${this.id}/cities/?page=${page}`)
//             .then((response) => {
//                 response.data.meta = this.meta;
//                 response.data.data = this.data;
//             //   this.meta = response.data.meta;
//             //   this.cities = response.data.data;
//               console.log(response.data.data);
//               console.log(response.data.meta);

//             });
//         } else {
//             this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries/${this.id}/cities`)
//             .then((response) => {
//               this.cities = response.data.data;
//             });
//         }
//       }
//     },
//   },

  methods: {
    getData() {
      this.$http
        .get("https://akademija.teltonika.lt/countries_api/api/countries/" + this.id + "/cities", {
            params: {
              search: this.search,
            },
          }
        )
        .then((response) => {
          this.cities = response.data.data;
        })
        .catch((errors) => {
          console.log(errors);
        });
    },

    // getData(page) {
    //     console.log(`Checking term ${this.search}`);
    //     this.$http.get(`https://akademija.teltonika.lt/countries_api/api/countries/${this.id}/cities&page=${page}&search=${this.search}`)
    //     .then(response => {
    //       console.log(response.data.results);
    //       this.countries = response.data.data
    //     })
    //     .catch(errors => {
    //       console.log(errors);
    //     })
    // },

    deleteCity(city) {
      let choice = confirm(
        `Are you sure you want to delete ${city.attributes.name}?`
      );

      if (choice) {
        let testCities = this.cities.filter((cty) => cty.id != city.id);
        this.cities = testCities;

        this.$http({
          method: "delete",
          url: `https://akademija.teltonika.lt/countries_api/api/countries/${this.id}/cities/${this.idCity}`,
        })
          .then((response) => {
            console.log(response.data.message);
          })
          .catch((errors) => {
            console.log(errors);
          });
      }
    },

    sortedArray() {
      if (this.descending == false) {
        this.descending = true;
        return this.cities.sort(
          (a, b) => b.attributes.population - a.attributes.population
        );
      } else {
        this.descending = false;
        return this.cities.sort(
          (a, b) => a.attributes.population - b.attributes.population
        );
      }
    },
  },

    mounted() {
      this.getData();
    },

  // mixins: [searchCity]
};
</script>