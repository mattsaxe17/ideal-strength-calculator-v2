<template>
  <v-app>
    <Nuxt />
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {}
  },
}
</script>

<style lang="scss">
#app {
  background: linear-gradient(105deg, var(--v-background-base) 0%, var(--v-background-darken3) 100%) !important;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: 100% 100%;
}
</style>
