<template>
  <div class="home-page-empty w-full flex flex-col justify-center items-center pt-20 md:pt-32">
    <svg 
      class="w-24 h-24 text-gray-200"
      mlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
    >
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z" />
    </svg>
    <span 
      v-text="'Take a look to our offices'"
      class="font-karla text-xl text-gray-300 mt-10"
    />
  </div>
</template>

<script>
export default {
  name: 'HomePageEmpty',
}
</script>
