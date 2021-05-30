<template>
  <a target="_blank" :href="href" title="Zur Navigation">
    <li
      class="col-span-1 bg-white hover:bg-gray-100 rounded-lg shadow p-6 pb-3"
    >
      <div class="w-full flex items-center justify-between space-x-6">
        <div class="flex-1 truncate">
          <div class="flex items-center space-x-3">
            <h3 class="lowercase text-gray-900 text-sm font-medium truncate">
              {{ gasStation.brand }}
            </h3>
            <span
              :class="{
                'bg-green-100 text-green-800': gasStation.isOpen,
                'bg-red-100 text-red-800': !gasStation.isOpen,
              }"
              class="lowercase lex-shrink-0 inline-block px-2 py-0.5 text-xs font-medium rounded-full"
              >{{ gasStation.isOpen ? "geöffnet" : "geschlossen" }}</span
            >
          </div>
          <p class="mt-1 text-gray-500 text-sm truncate lowercase">
            {{ gasStation.street }} {{ gasStation.houseNumber }}<br />{{
              gasStation.postCode
            }}
            {{ gasStation.place }}
          </p>
        </div>
      </div>

      <dl class="grid grid-cols-1 sm:grid-cols-1 pt-5">
        <div class="relative verflow-hidden h-9">
          <dt>
            <div class="absolute bg-gray-700 rounded-md p-1">
              <FlagIcon class="h-4 w-4 text-white" aria-hidden="true" />
            </div>
          </dt>
          <dd class="ml-8 flex items-baseline">
            <p class="text-l font-semibold text-gray-900">
              {{ gasStation.dist }}km
            </p>
          </dd>
        </div>
        <div class="relative verflow-hidden h-9">
          <dt>
            <div class="absolute bg-gray-700 rounded-md p-1">
              <CurrencyEuroIcon class="h-4 w-4 text-white" aria-hidden="true" />
            </div>
          </dt>
          <dd class="ml-8 flex items-baseline">
            <p class="text-l font-semibold text-gray-900">
              {{ gasStation[sortParams.type] }}
              <span class="text-gray-400 text-xs truncate capitalize">
                ({{ sortParams.type }})
              </span>
            </p>
          </dd>
        </div>
      </dl>
    </li>
  </a>
</template>

<script>
import { CurrencyEuroIcon, FlagIcon } from "@heroicons/vue/solid";
export default {
  components: { FlagIcon, CurrencyEuroIcon },
  props: ["gasStation", "sortParams", "geoPos"],
  data(props) {
    return {
      href:
        "https://www.google.com/maps/dir/" +
        (props.geoPos
          ? props.geoPos.latitude + "," + props.geoPos.longitude
          : "") +
        "/" +
        props.gasStation.lat +
        "," +
        props.gasStation.lng,
    };
  },
};
</script>