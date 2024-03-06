<template>
  <div v-if="character" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
    <div class="bg-white p-6 max-w-[1000px] mx-auto rounded-lg flex relative items-center">
      <div class="w-1/2 pr-4">
        <img :src="character.fullPortrait" :alt="character.title" class="w-full mb-4">
        <div class="flex items-center justify-center text-center">
          <span class="text-xl font-semibold mr-5">{{ character.displayName }}</span>
          <span @click="openYouTubeVideo" class="bg-red-500 text-white px-3 py-1 rounded-md flex items-center">Watch in Video</span>
        </div>
      </div>
      <div class="w-1/2 text-center">
        <div class="text-xl text-left pl-8 pb-2 font-semibold mr-5">Ability</div>
        <div v-if="character.abilities.length">
          <div class="px-4 mb-4">
            <div class="px-4 mb-4">
              <div v-for="(ability, index) in character.abilities" :key="index" class="mb-1 flex items-center rounded-lg shadow-md bg-black">
                <div class="p-4 min-w-[60px] max-w-[60px]">
                  <img :src="ability.displayIcon" :alt="ability.displayName" class="w-full h-auto object-contain">
                </div>
                <div class="py-4 pr-4 text-left text-white">
                  <p class="text-xl">{{ ability.displayName }}</p>
                  <p class="text-[9px]">{{ ability.description }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button @click="closeDialog" class="absolute top-0 left-full mt-1 ml-2 px-1 py-1 bg-red-500 text-white rounded-full flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    character: Object
  },
  methods: {
    openYouTubeVideo() {
      if (this.character.video) {
        window.open(this.character.video, '_blank');
      } else {
        console.error('Character video is null or undefined');
      }
    },
    closeDialog() {
      this.$emit('close');
    }
  }
};
</script>

<style scoped>
/* Add your custom styles here */
</style>
