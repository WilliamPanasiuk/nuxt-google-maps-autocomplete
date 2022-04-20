<template>
  <div>
    <Search
      @eventDataFromSearch="reloadDataMap"
    />
    <Map 
      :searchMapParams="searchMapParams" 
    />
  </div>
</template>
<script>
export default {
  data: () => ({
    searchMapParams:  {
      type: Object,
      required: true
    },
  }),
  methods: {
    reloadDataMap(payload) {
      this.searchMapParams = payload
    }
  }
}
</script>