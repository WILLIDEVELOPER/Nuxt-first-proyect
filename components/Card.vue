<template >
  <nuxt-link :to="{ path: nombre, query: { image: link }}" class=" col-span-1  flex flex-col items-center rounded-lg gap-[0.5rem] border border-[2px] ">
    <img :src="link" alt="" class="w-[10rem]">
    <p class="text-center text-white font-bold"> {{ nombre }}</p>
  </nuxt-link>
</template>
<script>
export default {
  name: "CardPage",
  props: {
    nombre: String,
    link: String
  }
}
</script>
<style >
  
</style>