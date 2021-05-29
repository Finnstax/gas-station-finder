<template>
  <div class="flex pb-6">
    <GeolocationButton @geoSuccessful="updateLatLong" class="pr-4" />
    <FilterGroup
      @filterEvent="updateParam"
      class="pr-4 w-1/6"
      :optionData="fuelOptions"
    />
    <FilterGroup
      @filterEvent="updateParam"
      class="pr-4 w-1/6"
      :optionData="rangeOptions"
    />
    <FilterGroup
      @filterEvent="updateParam"
      class="pr-4 w-1/6"
      :optionData="sortingOptions"
    />
    <Toggle class="ml-5 self-end mb-2" />
  </div>
  <ul class="grid grid-cols-1 py-6 gap-6 sm:grid-cols-2 lg:grid-cols-5">
    <GasStation
      v-for="gasStation in gasStations"
      :key="gasStation.id"
      :gasStation="gasStation"
      :sortParams="sortParams"
    />
  </ul>
</template>

<script>
import axios from "axios";
import {} from "@heroicons/vue/solid";
import GasStation from "./GasStation.vue";
import FilterGroup from "./FilterGroup.vue";
import Toggle from "./Toggle.vue";
import GeolocationButton from "./GeolocationButton.vue";

export default {
  components: {
    GasStation,
    FilterGroup,
    Toggle,
    GeolocationButton,
  },
  data() {
    return {
      apiParams: {
        lat: 53.5499492,
        long: 9.990664,
        rad: 25,
        sort: "dist",
        type: "all",
      },
      sortParams: {
        type: "e5",
        sort: "dist",
        rad: 5,
      },
      publicToken: process.env.VUE_APP_TOTALLY_NOT_AN_API_KEY,
      gasStations: [],
      fuelOptions: {
        label: "Kraftstoff",
        options: [
          //{ id: 1, name: "Alle anzeigen", value: "all", param: "type" },
          { id: 2, name: "E5", value: "e5", param: "type" },
          { id: 3, name: "E10", value: "e10", param: "type" },
          { id: 4, name: "Diesel", value: "diesel", param: "type" },
        ],
      },
      sortingOptions: {
        label: "Sortierung",
        options: [
          { id: 1, name: "Preis", value: "price", param: "sort" },
          { id: 2, name: "Entfernung", value: "dist", param: "sort" },
        ],
      },
      rangeOptions: {
        label: "Suchradius",
        options: [
          { id: 1, name: "5km", value: "5", param: "rad" },
          { id: 2, name: "10km", value: "10", param: "rad" },
          { id: 3, name: "15km", value: "15", param: "rad" },
          { id: 4, name: "20km", value: "20", param: "rad" },
          { id: 5, name: "25km", value: "25", param: "rad" },
        ],
      },
    };
  },

  methods: {
    updateParam(param, value) {
      this.sortParams[param] = value;
      //this.getGasStationData();
    },
    updateLatLong(pos) {
      this.apiParams.lat = pos.latitude;
      this.apiParams.long = pos.longitude;
      this.getGasStationData();
    },
    getGasStationData() {
      axios
        .get(
          "https://creativecommons.tankerkoenig.de/json/list.php?lat=" +
            this.apiParams.lat +
            "&lng=" +
            this.apiParams.long +
            "&rad=" +
            this.apiParams.rad +
            "&sort=" +
            this.apiParams.sort +
            "&type=" +
            this.apiParams.type +
            "&apikey=" +
            this.publicToken
        )
        .then((response) => (this.gasStations = response.data.stations));
    },
  },
  mounted() {
    this.getGasStationData();
  },
};
</script>
