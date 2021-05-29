<template>
  <li class="col-span-1 bg-white rounded-lg shadow divide-y divide-gray-200">
    <div class="w-full flex items-center justify-between p-6 space-x-6">
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
  </li>
</template>

<script>
export default {
  components: {},
  props: ["gasStation"],
  data() {
    return {};
  },
};
</script>