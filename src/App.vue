<template>
  <div>
    <Disclosure as="nav" class="bg-gray-800">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <img
                class="h-8 w-8"
                src="https://tailwindui.com/img/logos/workflow-mark-indigo-500.svg"
                alt="Workflow"
              />
            </div>
            <div class="hidden md:block">
              <div class="ml-10 flex items-baseline space-x-4">
                <template v-for="(item, itemIdx) in navigation" :key="item">
                  <template v-if="itemIdx === 0">
                    <a
                      href="#"
                      class="bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium"
                      >{{ item }}</a
                    >
                  </template>
                  <a
                    v-else
                    href="#"
                    class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium"
                    >{{ item }}</a
                  >
                </template>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Disclosure>

    <header class="bg-white shadow">
      <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
        <h1 class="text-3xl font-bold text-gray-900">Tankstellen</h1>
      </div>
    </header>
    <main>
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <GasStationList />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { Disclosure } from "@headlessui/vue";
import {} from "@heroicons/vue/outline";
import GasStationList from "./components/GasStationList.vue";

const navigation = ["Tankstellen", "Über"];
export default {
  components: {
    Disclosure,
    GasStationList,
  },
  setup() {
    return {
      navigation,
    };
  },
};
</script>