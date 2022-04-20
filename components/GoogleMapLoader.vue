<template>
  <div id="map" ref="googleMap"></div>
</template>
<script>
import MarkerClusterer from "@googlemaps/markerclustererplus";

export default {
  props: {
    places: {
      type: Array,
      required: true,
    },
    center: {
      type: Object,
      required: false,
    },
  },
  data: () => ({
    map: {
      type: Object,
      required: false,
    },
    mapConfig: {
      zoom: 6,
      center: {
        lat: 52,
        lng: 19,
      },
      mapTypeId: "roadmap",
      streetViewControl: false,
      styles: [
        {
          elementType: "geometry",
          stylers: [
            {
              color: "#f5f5f5",
            },
          ],
        },
        {
          elementType: "labels.icon",
          stylers: [
            {
              visibility: "off",
            },
          ],
        },
        {
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#616161",
            },
          ],
        },
        {
          elementType: "labels.text.stroke",
          stylers: [
            {
              color: "#f5f5f5",
            },
          ],
        },
        {
          featureType: "administrative.country",
          elementType: "geometry.stroke",
          stylers: [
            {
              color: "#bdbdbd",
            },
            {
              weight: 1,
            },
          ],
        },
        {
          featureType: "administrative.land_parcel",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#bdbdbd",
            },
          ],
        },
        {
          featureType: "administrative.locality",
          stylers: [
            {
              visibility: "on",
            },
          ],
        },
        {
          featureType: "administrative.province",
          elementType: "geometry",
          stylers: [
            {
              color: "#9e9e9e",
            },
          ],
        },
        {
          featureType: "landscape.man_made",
          elementType: "geometry",
          stylers: [
            {
              color: "#d6d6d6",
            },
          ],
        },
        {
          featureType: "poi",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#757575",
            },
          ],
        },
        {
          featureType: "poi.place_of_worship",
          stylers: [
            {
              color: "#7a7a7a",
            },
          ],
        },
        {
          featureType: "road",
          elementType: "geometry",
          stylers: [
            {
              color: "#ffffff",
            },
          ],
        },
        {
          featureType: "road.arterial",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#757575",
            },
          ],
        },
        {
          featureType: "road.highway",
          elementType: "geometry",
          stylers: [
            {
              color: "#dadada",
            },
          ],
        },
        {
          featureType: "road.highway",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#616161",
            },
          ],
        },
        {
          featureType: "road.local",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#9e9e9e",
            },
          ],
        },
        {
          featureType: "transit.line",
          elementType: "geometry",
          stylers: [
            {
              color: "#e5e5e5",
            },
          ],
        },
        {
          featureType: "transit.station",
          elementType: "geometry",
          stylers: [
            {
              color: "#eeeeee",
            },
          ],
        },
        {
          featureType: "water",
          elementType: "geometry",
          stylers: [
            {
              color: "#e3e3e3",
            },
          ],
        },
        {
          featureType: "water",
          elementType: "labels.text.fill",
          stylers: [
            {
              color: "#9e9e9e",
            },
          ],
        },
      ],
    },
    markers: {
      type: Array,
      required: false,
    },
    clusters: {
      type: Array,
      required: false,
    },
  }),

  watch: {
    center(newVal, oldVal) {
      if (this.center?.lat && this.center?.lng) {
        this.map.setZoom(10);
        this.map.setCenter(
          new google.maps.LatLng(this.center.lat, this.center.lng)
        );
      }
    },
  },
  mounted() {
    console.log(window.google);
    if (window.google) {
      const mapContainer = this.$refs.googleMap;
      this.map = new window.google.maps.Map(mapContainer, this.mapConfig);

      // markers
      this.markers = this.places.map((place, i) => {
        var marker = new google.maps.Marker({
          position: place,
          icon: "/img/map_pin.svg",
        });

        return marker;
      });

      // clusters
      const clustersOptions = {
        gridSize: 50,
        minZoom: 10,
        maxZoom: 15,
        color: "purple",
        averageCenter: true,
        styles: [
          {
            anchorText: [7, 0],
            fontWeight: 800,
            textColor: "#ff0000",
            url: "/img/map_cluster.svg",
            anchorIcon: [40, 20],
            width: 40,
            height: 40,
          },
        ],
      };
      this.clusters = new MarkerClusterer(
        this.map,
        this.markers,
        clustersOptions
      );
    }
  },
};
</script>
<style>
#map {
  height: calc(100vh - 10rem);
  width: 100%;
}
</style>