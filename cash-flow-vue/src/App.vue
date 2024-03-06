<template>
  <div class="w-full">
    <header class="w-full text-white">
      <div class="w-full mx-auto">
        <div class="relative w-full">
          <img src="./media/header.png" class="w-full" alt="Banner Image">
          <div class="absolute bg-black-400 bottom-0 w-full flex items-center justify-center bg-black">
            <nav class="hidden md:block py-2 px-5">
              <button @click="resetFilter" :class="{ 'border-b-[3px] border-solid py-2 border-red-500': !selectedRole }" class="border-b-[3px] border-solid py-2 border-transparent mr-4 cursor-pointer"><a>ALL</a></button>
              <a v-for="role in uniqueRoles" :key="role" @click="filterByRole(role)" 
                :class="{ 'border-b-[3px] px-2 py-2 border-solid border-red-500': selectedRole === role }" 
                class="mr-4 px-2 py-2 cursor-pointer">{{ role.toUpperCase() }}</a>
            </nav>
          </div>
        </div>
      </div>
    </header>

    <div class="container mx-auto mt-8 px-[50px]">
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-10">
        <div v-for="character in displayedCharacters" :key="character.id" class="overflow-hidden" @click="openDialog(character)">
          <img :src="character.fullPortrait" :alt="character.title" class="w-full bg-gray-400 h-[360px] object-cover border-b-[5px] border-solid border-red-500">
          <div class="p-4">
            <h2 class="text-lg text-center font-semibold mb-2 whitespace-nowrap">{{ character.displayName.toUpperCase() }}</h2>
          </div>
        </div>
      </div>

      <CharacterDialogue :character="selectedCharacter" v-if="dialogVisible" @close="closeDialog" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CharacterDialogue from '@/views/CharacterDialogue.vue'; // Assuming you have a component for the dialogue

export default {
  data() {
    return {
      characters: [],
      searchQuery: '',
      filteredCharacters: [],
      selectedRole: null,
      selectedCharacter: null,
      dialogVisible: false
    };
  },
  props: {
    character: Object
  },
  components: {
    CharacterDialogue // Register the component
  },
  computed: {
    uniqueRoles() {
      // Extract unique roles from characters
      return [...new Set(this.characters.map(character => character.role))];
    },
    displayedCharacters() {
      return this.filteredCharacters.length ? this.filteredCharacters : this.characters;
    }
  },
  methods: {
    filterByRole(role) {
      // Filter characters based on selected role
      this.filteredCharacters = this.characters.filter(character => character.role === role);
      this.selectedRole = role; // Set selected role
    },
    resetFilter() {
      // Reset filtered characters to all characters
      this.filteredCharacters = this.characters;
      this.selectedRole = null; // Reset selected role
    },
    filterCharacters() {
      const query = this.searchQuery.toLowerCase().trim();
      if (query) {
        this.filteredCharacters = this.characters.filter(character => 
          character.displayName.toLowerCase().includes(query) || 
          character.description.toLowerCase().includes(query)
        );
      } else {
        this.filteredCharacters = this.characters; // Show all characters if no search query
      }
    },
    resetSearch() {
      this.searchQuery = ''; // Reset search query
      this.filteredCharacters = this.characters; // Reset filtered characters to all characters
    },
    openDialog(character) {
      console.log("Opening dialog with character:", character);
      this.selectedCharacter = character;
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
    }
  },
  mounted() {
    axios.get('https://staging.ina17.com/data.json')
      .then(response => {
        this.characters = response.data;
        this.filteredCharacters = this.characters; // Initialize filtered characters with all characters
      })
      .catch(error => {
        console.error('Error fetching data:', error);
      });
  }
};
</script>

<style>
/* Add your custom styles here */
</style>
