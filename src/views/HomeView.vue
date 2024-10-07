<template>
  <login v-if="loginactive" @close="loginactive = false" />
  <h1 class="text-center text-3xl font-bold my-4">Club List</h1>
  <div class="flex justify-center">
    <input
      id="searchClub"
      type="text"
      placeholder="Search"
      class="bg-gray-400 bg-opacity-50 text-black text-xl rounded-lg p-2 mb-4 md:w-full w-3/4"
      v-model="searchRef"
      @input="updateFilteredClubs()"
    />
    <label for="searchClub" class="sr-only"> Club List </label>
  </div>
  <div id="clublist" class="flex flex-col md:flex-row flex-wrap items-center">
    <div
      v-for="club in filteredClubs"
      :key="club"
      class="bg-gradient-to-br from-primary to-accent text-white text-lg font-semibold p-3 rounded-lg border-2 border-gray-700 mb-6 lg:w-1/4 md:w-1/2 w-3/4 text-center"
    >
      <h2 class="text-black">
        {{ club["Club Name"] }}
      </h2>
      <button
        class="btn"
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
