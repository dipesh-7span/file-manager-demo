<template>
  <div>
    <!-- header  -->
    <TheNavbar />

    <!-- Main content -->
    <div class="w-full z-0 relative">
      <FileUploadManager />
    </div>
  </div>
</template>

<script>
import TheNavbar from './components/TheNavbar.vue'
import FileUploadManager from './components/FileUploadManger.vue'
export default {
  name: 'App',
  components: {
    TheNavbar,
    FileUploadManager,
  },

  data() {
    return {}
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
