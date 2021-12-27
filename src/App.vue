<template>
  <div id="app">
      <!-- table component -->
      <user-table></user-table>
  </div>
</template>

<script>
import UserTable from './components/UserTable.vue';

export default {
  name: 'App',
  components: {
    UserTable,
  }
}
</script>
<style src="@/assets/tailwind.css"></style>
