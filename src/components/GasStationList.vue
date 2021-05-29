<template>
  <div class="flex pb-6">
    <GeolocationButton class="pr-4" />
    <FilterGroup class="pr-4 w-1/6" :optionData="fuelOptions" />
    <FilterGroup class="pr-4 w-1/6" :optionData="rangeOptions" />
    <FilterGroup class="pr-4 w-1/6" :optionData="sortingOptions" />
    <Toggle class="ml-5 self-end mb-2" />
  </div>
  <ul class="grid grid-cols-1 py-6 gap-6 sm:grid-cols-2 lg:grid-cols-4">
    <GasStation
      v-for="gasStation in gasStations"
      :key="gasStation.id"
      :gasStation="gasStation"
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
      publicToken: process.env.VUE_APP_TOTALLY_NOT_AN_API_KEY,
      gasStations: [],
      fuelOptions: {
        label: "Treibstoff",
        options: [
          { id: 1, name: "Alle anzeigen" },
          { id: 2, name: "E5" },
          { id: 3, name: "E10" },
          { id: 4, name: "Diesel" },
        ],
      },
      sortingOptions: {
        label: "Sortierung",
        options: [
          { id: 1, name: "Preis" },
          { id: 2, name: "Entfernung" },
        ],
      },
      rangeOptions: {
        label: "Suchradius",
        options: [
          { id: 1, name: "5km" },
          { id: 2, name: "10km" },
          { id: 3, name: "15km" },
          { id: 4, name: "20km" },
          { id: 5, name: "25km" },
        ],
      },
    };
  },

  methods: {
    getGasStationData() {
      axios
        .get(
          "https://creativecommons.tankerkoenig.de/json/list.php?lat=52.52099975265203&lng=13.43803882598877&rad=4&sort=price&type=diesel&apikey=" +
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
