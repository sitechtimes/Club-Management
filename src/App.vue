<script setup></script>

<template>
  <main class="p-3">
    <header class="flex items-center max-h-screen">
      <div id="navbar" class="flex items-center justify-start">
        <RouterLink to="/" class="text-lg font-bold underline">
          Home
        </RouterLink>
      </div>
    </header>

    <RouterView />
  </main>
</template>
