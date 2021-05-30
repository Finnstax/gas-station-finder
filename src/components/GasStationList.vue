<template>
  <div class="flex gap-2 gap-y-3 xl:gap-5 flex-wrap">
    <GeolocationButton class="w-screen md:w-auto" @geoSuccessful="updateLatLong" />
    <FilterGroup
      class="w-24"
      @filterEvent="updateParam"
      :optionData="fuelOptions"
    />
    <FilterGroup
      class="w-24"
      @filterEvent="updateParam"
      :optionData="rangeOptions"
    />
    <FilterGroup
      class="w-36"
      @filterEvent="updateParam"
      :optionData="sortingOptions"
    />
    <Toggle @toggleVisiblity="updateParam" />
  </div>
  <ul
    class="grid grid-cols-1 py-6 gap-6 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5"
  >
    <GasStation
      v-for="gasStation in filteredGasStations.slice(0, this.limit)"
      :key="gasStation.id"
      :gasStation="gasStation"
      :sortParams="sortParams"
    />
  </ul>
  <div class="flex justify-center pt-10">
    <button
      v-if="limit < this.filteredGasStations.length"
      @click="limit += 10"
      type="button"
      class="center inline-flex items-center px-4 py-2 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      <PlusCircleIcon class="-ml-1 mr-2 h-5 w-5" aria-hidden="true" />
      Mehr anzeigen
    </button>
  </div>
</template>

<script>
import axios from "axios";
import { PlusCircleIcon } from "@heroicons/vue/solid";
import GasStation from "./GasStation.vue";
import FilterGroup from "./FilterGroup.vue";
import Toggle from "./Toggle.vue";
import GeolocationButton from "./GeolocationButton.vue";

export default {
  components: {
    GasStation,
    FilterGroup,
    Toggle,
    PlusCircleIcon,
    GeolocationButton,
  },
  data() {
    return {
      limit: 20,
      apiParams: {
        lat: 53.5499492,
        long: 9.990664,
        rad: 25,
        sort: "dist",
        type: "all",
      },
      sortParams: {
        type: "e5",
        sort: "price",
        rad: 5,
        showOnlyOpen: true,
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
          { id: 2, name: "Preis", value: "price", param: "sort" },
          { id: 1, name: "Entfernung", value: "dist", param: "sort" },
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

  computed: {
    filteredGasStations() {
      return this.filterAndSortGasStations();
    },
  },
  methods: {
    updateParam(param, value) {
      this.sortParams[param] = value;
    },
    filterAndSortGasStations() {
      var filteredStations = this.gasStations;
      Object.entries(this.sortParams).forEach(([key, value]) => {
        switch (key) {
          case "rad":
            filteredStations = filteredStations.filter((x) => x.dist < value);
            break;
          case "sort":
            if (value == "dist") {
              filteredStations = filteredStations.sort(
                (a, b) => a[value] - b[value]
              );
            }
            if (value == "price") {
              filteredStations = filteredStations.filter(
                (x) => x[this.sortParams.type] !== null
              );
              filteredStations = filteredStations.sort(
                (a, b) => a[this.sortParams.type] - b[this.sortParams.type]
              );
            }
            break;
          case "showOnlyOpen":
            if (value) {
              filteredStations = filteredStations.filter((x) => x.isOpen);
            }
            break;
        }
      });
      return filteredStations;
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
