<template>
  <form @submit.prevent>
    <input 
      :name="'searchPlaceTextField'"
      :type="'text'"
      :placeholder="'Find place'"
      />
  </form>
</template>
<script>
export default {
  data: () => ({    
    formData: {
      geolocation: {
        lat: Number,
        lng: Number
      }
    }
  }),

  watch: {
    formData: {
      deep: true,
      async handler(newVal, oldVal) {
        let emitParamsToMap = {}
        emitParamsToMap.geolocation = {
          lat: this.formData.geolocation.lat,
          lng: this.formData.geolocation.lng,
        }
        this.$emit('eventDataFromSearch', emitParamsToMap)
      }
    },   
  },
  mounted() {

    // Google Autocomplete
    const autocompleteOptions = {
      componentRestrictions: { country: 'pl' },
      region: 'pl',
      types: ['geocode']
    }

    // Google Autocomplete bind with DOM element
    var searchPlaceInput = document.getElementsByName('searchPlaceTextField')[0]
    var autocomplete = new window.google.maps.places.Autocomplete(
      searchPlaceInput,
      autocompleteOptions
    )

    // listen autocomplete
    autocomplete.addListener('place_changed', () => {      
      const place = autocomplete.getPlace()
      this.formData.geolocation.lat = place.geometry?.location.lat()
      this.formData.geolocation.lng = place.geometry?.location.lng()
    })
  },
}
</script>
<style scoped>
form{
  width:100%;
  margin:2rem
}
input[type="text"]{
  width:calc(100% - 6rem);
  padding:1rem;
}
</style>