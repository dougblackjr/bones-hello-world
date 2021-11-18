<template>
  <div class="container m-auto p-16">
    <h1 class="text-2xl">Welcome to Bones</h1>
    <h2 class="text-xl">
      Here you can test your Bones implementation.
    </h2>
    <h3>Steps</h3>
    <ul class="flex flex-col text-left">
      <li>1. Add your environment variables to the .env</li>
      <li>2. Hit the Call button</li>
      <li>3. Add your variables to test this.</li>
    </ul>
    <BonesComponent />
  </div>
</template>

<script>
  import BonesComponent from './BonesComponent.vue'
export default {
  name: 'HelloWorld',
  components: {
    BonesComponent
  },
  props: {
    msg: String
  }
}
</script>