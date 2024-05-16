<script setup></script>

<template>
  <div>

    <div class="min-h-screen bg-cyan-900 flex flex-col justify-center align-middle p-6 space-y-12">
      <h1 class="text-5xl font-extrabold text-blue-100 text-center">Welcome to Question and Answer game</h1>
      <div class="flex justify-center">
        <NuxtLink
          class="text-lg px-5 font-medium py-2 border border-gray-500 rounded-md bg-blue-500 text-white"
          to="./questions"
        >
        Let's get started!
        </NuxtLink>
      </div>
    </div>

  </div>
</template>

<style>
* {
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
</style>
