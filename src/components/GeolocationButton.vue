<!-- This example requires Tailwind CSS v2.0+ -->
<template>
  <div>
    <label
      id="headlessui-listbox-label-2"
      class="block text-sm font-medium text-gray-700"
      >Standort</label
    >

    <button
      v-if="!location && !errorStr"
      @click="locateMe()"
      type="button"
      class="mt-1 inline-flex items-center px-4 py-2 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      Standort festlegen
      <LocationMarkerIcon class="ml-2 -mr-1 h-5 w-5" aria-hidden="true" />
    </button>

    <button
      :disabled="true"
      v-if="errorStr"
      @click="locateMe"
      type="button"
      class="mt-1 inline-flex items-center px-4 py-2 border opacity-50 cursor-default hover:none border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-red-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      Fehlerhafter Standort
      <ExclamationCircleIcon class="ml-2 -mr-1 h-5 w-5" aria-hidden="true" />
    </button>

    <button
      :disabled="true"
      v-if="location"
      type="button"
      class="mt-1 inline-flex hover:none cursor-default items-center px-4 py-2 opacity-50 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      Standort festgelegt
      <CheckCircleIcon class="ml-2 -mr-1 h-5 w-5" aria-hidden="true" />
    </button>
  </div>
</template>

<script>
import {
  LocationMarkerIcon,
  ExclamationCircleIcon,
  CheckCircleIcon,
} from "@heroicons/vue/solid";

export default {
  components: {
    LocationMarkerIcon,
    ExclamationCircleIcon,
    CheckCircleIcon,
  },
  data() {
    return {
      location: null,
      gettingLocation: false,
      errorStr: null,
    };
  },
  methods: {
    async getLocation() {
      return new Promise((resolve, reject) => {
        if (!("geolocation" in navigator)) {
          reject(new Error("Geolocation is not available."));
        }

        navigator.geolocation.getCurrentPosition(
          (pos) => {
            this.$emit("geoSuccessful", pos.coords);
            resolve(pos);
          },
          (err) => {
            reject(err);
          }
        );
      });
    },
    async locateMe() {
      this.gettingLocation = true;
      try {
        this.gettingLocation = false;
        this.location = await this.getLocation();
      } catch (e) {
        this.gettingLocation = false;
        this.errorStr = e.message;
      }
    },
  },
};
</script>