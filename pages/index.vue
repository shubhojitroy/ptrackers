<template>
  <div>
    <div class="container">
      <section class="canvas">
        <h1 class="title is-size-2 is-size-3-mobile">
          PM Capital Global Opportunities Fund Limited<br>PM Capital <span style="color: red;">GO 2025</span> Limited
        </h1>
        <p>
          Portfolio Tracking Exchangeable Redeemable Securities (Converting Security) (known as ‘<span style="color: red;">PTrackERS</span>’)
        </p>
        <div class="space"></div>
        <div class="alert-bound">
          <p>
            The PTrackERS ("Offer") is being made to any person who has a registered address in Australia or New Zealand.
          </p>
          <p>
            Due to legal restrictions, access to these webpages is only intended for access by persons within Australia and New Zealand. These webpages do not constitute an offer of securities in any place in which, or to any person to whom, it would not be lawful to make such an offer.
          </p>
          <div><!--class="columns"-->
            <div><!--class="column is-two-thirds-tablet is-half-desktop"-->
              <form novalidate @submit.prevent="submitForm">
                <b-field label="Country of Primary Residence" :type="countryType" :message="countryMessage">
                  <b-autocomplete v-model="country" field="name" placeholder="Select a Country" :data="filteredCountries" open-on-focus keep-first
                    icon="earth" @select="option => selected = option" @blur="$v.country.$touch()">
                    <template slot="empty">No results found</template>
                  </b-autocomplete>
                </b-field>
                <br>
                <div class="control">
                <button type="submit" class="button is-primary">
                  Submit&nbsp;<span class="icon"><i class="mdi mdi-login"></i></span>
                </button>
                </div>
              </form>
            </div>
          </div>
        </div>
       </section>
    </div>
  </div>
</template>

<script>

import { countries } from "~/assets/js/countries.js"
import { required } from 'vuelidate/lib/validators'

export default {
  components: {
  },
  data () {
    return {
      name: "",
      country: "",
      selected: null,
      countries: countries.items
    }
  },
  validations: {
    country: {
      required
    }
  },
  computed: {
    filteredCountries() {
      return this.countries.filter((option) => {
        return option.name
          .toString()
          .toLowerCase()
          .indexOf(this.country.toLowerCase()) >= 0
        })
    },
    countryType() {
      return this.$v.country.$error ? "is-danger" : ""
    },
    countryMessage() {
     return this.$v.country.$error ? "Country is required" : ""
    }
  },
  methods: {
    submitForm () {
      this.$v.$touch();
      if (!this.$v.$error) {
        const validCountry = this.selected.code === 'AUS' || this.selected.code === 'NZL' ? '/offer-details' : '/eligibility-notice';
        this.$router.push(validCountry);
      }
    }
  }
}
</script>

<style scoped>
.canvas {
  z-index: 50;
  padding: 1rem;
  background: #fff 50%;
  background-size: cover;
  border-radius: 1px;
  box-shadow: 0 0 1px rgba(0, 0, 0, 0.1), 0 2px 4px rgba(0, 0, 0, 0.02);
  transition: all 0.5s ease;
}
h4 {
  font-size: 1.33rem;
  margin: 0.75em 0;
  font-weight: 500;
}
ul {
  list-style-type: disc;
}
li {
  line-height: 1.5;
  padding: 0.5rem 1rem;
  margin: 0 1.25em;
}
p {
  line-height: 1.75rem;
}
.space {
  display: block;
  width: 100%;
  height: 1.5em;
}
.alert-bound {
  width: 100%;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 1px solid rgba(0, 0, 0, 0.125);
  border-radius: 0.25rem;
  padding: 1.0rem;

}
.footnote {
  font-size: 0.75rem;
  line-height: 1.2em;
}
</style>
