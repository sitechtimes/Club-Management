<template>
  <div
    @click.self="$emit('close')"
    id="backgroundLogin"
    class="fixed inset-0 w-full h-full bg-black bg-opacity-50 flex items-center justify-center"
  >
    <div
      id="loginPop"
      class="flex flex-col items-center justify-center bg-gray-800 rounded-lg p-4 w-80 animate-grow"
    >
      <label class="text-4xl text-white font-bold mb-4"
        >Passkey{{ loginInfo.currentClub["Pass"] }}</label
      >
      <form
        v-on:submit.prevent="login"
        class="flex flex-row items-center w-full"
      >
        <input
          id="passEnter"
          v-model="passkey"
          type="password"
          class="bg-gray-400 bg-opacity-50 text-white text-xl rounded-lg p-2 mx-1 w-3/4 border-2 border-black"
        />
        <button type="submit" class="btn">Login</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useLoginInfo } from "../stores/loginInfo";
import { useRouter } from "vue-router";

const router = useRouter();
const passkey = ref("");
const loginInfo = useLoginInfo();

function login() {
  if (passkey.value === loginInfo.currentClub["Pass"]) {
    loginInfo.loggedIn = loginInfo.currentClub["Club Name"];
    passkey.value = "";
    router.push(`/${loginInfo.currentClub["Club Name"]}`);
  } else {
    alert("Incorrect Passkey");
  }
}
</script>
