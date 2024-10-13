<template>
  <login v-if="loginactive" @close="loginactive = false" />
  <header class="text-center my-6">
    <h1 class="text-4xl font-extrabold text-gray-800 mb-6">Club List</h1>
    <div class="flex justify-center">
      <input
        id="searchClub"
        type="search"
        placeholder="Search for a club..."
        class="py-2 px-4 rounded-lg shadow-lg bg-white bg-opacity-75 text-gray-700 text-xl mb-6 focus:outline-none focus:ring-2 focus:ring-primary w-full max-w-lg"
        v-model="searchRef"
        @input="updateFilteredClubs()"
      />
    </div>
  </header>
  <div
    id="clublist"
    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 p-4"
  >
    <div
      v-for="club in filteredClubs"
      :key="club"
      class="bg-white shadow-md hover:shadow-lg transition-shadow duration-300 rounded-lg border border-gray-200 p-6"
    >
      <h2 class="text-2xl font-bold text-primary mb-2">
        {{ club["Club Name"] }}
      </h2>
      <p class="text-gray-600">
        {{ club.Description || "No description available." }}
      </p>
      <button
        class="mt-4 bg-primary hover:bg-accent text-white font-semibold py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-accent transition duration-200"
        @click="
          loginactive = true;
          updateCurrentClub(club);
        "
      >
        Login as President/Advisor
      </button>
    </div>
  </div>
</template>

<script setup>
import clubs from "../assets/data.json";
import { ref } from "vue";
import login from "../components/login.vue";
import { useLoginInfo } from "../stores/loginInfo";

const loginInfo = useLoginInfo();
const searchRef = ref("");
const filteredClubs = ref(clubs);

function updateCurrentClub(clubName) {
  loginInfo.currentClub = clubName;
}

function updateFilteredClubs() {
  filteredClubs.value = clubs.filter((club) =>
    club["Club Name"].toLowerCase().includes(searchRef.value.toLowerCase())
  );
}

const loginactive = ref(false);
</script>
