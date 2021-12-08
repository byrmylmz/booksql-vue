
<template>
  <div id="app" class="font-sans antialiased text-black">
    <div id="nav" class="bg-purple-900 ">
      <div class="container mx-auto flex items-center justify-between py-6 px-40 ">
        <div >
          <a href="/"><img src="./assets/logo.svg" alt="logo"></a>
        </div>
        <div >
          <router-link to="/" class="text-white hover:text-grey-light">Home</router-link>
          <router-link to="/about" class="text-white hover:text-grey-light pl-6">About</router-link>
        </div>
      </div>
    </div>

    <router-view/>
  </div>
</template>
