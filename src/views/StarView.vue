<template>
  <div id="app" class="container mx-auto pr-4 pl-4">
    <h1 class="font-bold text-2xl md:text-4xl mb-10">Profile Browser</h1>
    <div class="flex" v-if="isDesktop">
      <div class="border-r border-gray-200 dark:border-gray-700">
        <nav class="flex flex-col space-y-2" aria-label="Tabs" role="tablist" data-hs-tabs-vertical="true">
          <button v-for="(star, index) in stars" :key="index" :class="star.isActive ? 'hs-tab-active:border-blue-500 hs-tab-active:text-blue-600 dark:hs-tab-active:text-blue-600 py-1 pr-4 inline-flex items-center gap-2 border-r-[3px] border-transparent text-sm whitespace-nowrap text-gray-500 hover:text-blue-600 active' : 'hs-tab-active:border-blue-500 hs-tab-active:text-blue-600 dark:hs-tab-active:text-blue-600 py-1 pr-4 inline-flex items-center gap-2 border-r-[3px] border-transparent text-sm whitespace-nowrap text-gray-500 hover:text-blue-600 dark:hover:text-gray-300'" :id="'vertical-tab-with-border-item-' + (index + 1)" :data-hs-tab="'#vertical-tab-with-border-' + (index + 1)" :aria-controls="'vertical-tab-with-border-' + (index + 1)" :role="'tab'" @click="activateTab(star)">
            {{ star.name }}
          </button>
        </nav>
      </div>

      <div class="ml-3">
        <div v-for="(star, index) in stars" :key="index" :id="'vertical-tab-with-border-' + (index + 1)" :class="[star.isActive ? '' : 'hidden']" :role="'tabpanel'" :aria-labelledby="'vertical-tab-with-border-item-' + (index + 1)">
          <div class="flex">
            <img :src="star.image" class="mr-4" style="width: 200px;" alt="profil picture">
            <p class="text-gray-500 dark:text-gray-400">
              <em class="font-semibold text-gray-800 dark:text-gray-200">{{ star.description }}</em>
            </p>
          </div>
        </div>
      </div>
    </div>

    <div id="accordion-collapse" data-accordion="collapse" v-else>
      <template v-for="(star, index) in stars" :key="index">
        <h2 :id="`accordion-collapse-heading-${index+1}`">
          <button type="button"
            class="flex items-center justify-between w-full p-5 font-medium text-left text-gray-500 border border-b-0 border-gray-200 rounded-t-xl focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-800 dark:border-gray-700 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800"
            :data-accordion-target="`#accordion-collapse-body-${index+1}`"
            :aria-expanded="index === 0"
            :aria-controls="`accordion-collapse-body-${index+1}`"
            @click="toggleAccordion(index)"
          >
            <span>{{ star.name }}</span>
            <svg data-accordion-icon class="w-6 h-6 rotate-180 shrink-0" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd"></path>
            </svg>
          </button>
        </h2>
        <div
          :id="`accordion-collapse-body-${index+1}`"
          class="hidden"
          :aria-labelledby="`accordion-collapse-heading-${index+1}`"
        >
          <div class="p-5 border border-b-0 border-gray-200 dark:border-gray-700 dark:bg-gray-900">
            <img :src="star.image" class="mx-auto" style="width: 200px;" alt="profil picture">
            <br>
            <p class="mb-2 text-gray-500 dark:text-gray-400">{{ star.description }}</p>
            <p class="text-gray-500 dark:text-gray-400">{{ star.image }}</p>
          </div>
        </div>
      </template>
    </div>

  </div>
</template>

<script>
//import { onMounted } from 'vue'
import { initFlowbite, initAccordions, } from 'flowbite'

const axios = require('axios')



export default {
  name: "app",
  data() {
    return {
      stars: [],
      isDesktop: false
    };
  },

  computed: {},
  mounted() {
    initFlowbite();
    initAccordions();
    this.loadData();
    if (window.innerWidth >= 700) {
      this.isDesktop = true
    }
  },
  watch: {
    
  },

  methods: {
    loadData(){
      axios.get('http://127.0.0.1:8000/api/getAllStar').then(response => {
        this.stars = response.data
      })
    },
    activateTab(activeTab) {
      this.stars.forEach(star => {
        star.isActive = (star.name === activeTab.name);
      });
    },
    toggleAccordion(index) {
      const collapseBody = document.getElementById(`accordion-collapse-body-${index+1}`);
      if (collapseBody.classList.contains("hidden")) {
        collapseBody.classList.remove("hidden");
        collapseBody.classList.add("open");
      } else {
        collapseBody.classList.remove("open");
        collapseBody.classList.add("hidden");
      }
    }
  },
};
</script>