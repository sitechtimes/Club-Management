<template>
  <div
    @click.self="global.toggleLoginPage()"
    id="backgroundLogin"
    class="fixed inset-0 w-full h-full bg-gray-900 bg-opacity-50 flex items-center justify-center"
  >
    <div
      id="loginPop"
      class="bg-gray-800 rounded-lg p-4 w-96 animate-grow shadow-2xl"
    >
      <h2 class="text-4xl text-white font-bold mb-6">
        Passkey: {{ loginInfo.currentClub["Pass"] }}
      </h2>
      <form v-on:submit.prevent="login" class="flex flex-col w-full">
        <input
          id="passEnter"
          v-model="passkey"
          type="password"
          class="bg-gray-400 bg-opacity-50 text-white text-xl rounded-lg p-2 w-full border-2 border-black mb-4"
        />
        <button
          type="submit"
          class="btn bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg"
        >
          Login
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useLoginInfo } from "../stores/loginInfo";
import { useRouter } from "vue-router";
import { useGlobal } from "@/stores/global";

const router = useRouter();
const passkey = ref("");
const loginInfo = useLoginInfo();
const global = useGlobal();

function login() {
  if (passkey.value === loginInfo.currentClub["Pass"]) {
    loginInfo.loggedIn = loginInfo.currentClub["Club Name"];
    passkey.value = "";
    global.toggleLoginPage();
    router.push(`/${loginInfo.currentClub["Club Name"]}`);
  } else {
    alert("Incorrect Passkey");
  }
}

document.addEventListener("keydown", (e) => {
  if (e.key === "Escape") {
    global.toggleLoginPage();
  }
});
</script>
