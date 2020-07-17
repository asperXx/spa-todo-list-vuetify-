<template>
  <div>
    <v-app-bar color="primary" class="pl-4 pr-4" dark>
      <v-icon>fa-dragon</v-icon>
      <v-spacer></v-spacer>

      <v-toolbar-title>
        <router-link to="/" tag="span" class="pointer">
          ToDo List
        </router-link>
        </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn text height="100%" exact
      :to="'/'" 
      class="mr-2"
      >Create
      </v-btn>

      <v-btn text height="100%" exact
      :to="'/list'"
      >List</v-btn>
    </v-app-bar>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.pointer {
  cursor: pointer;
}
</style>
