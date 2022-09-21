<template>
  <v-app>
    <v-row>
      <v-col class="pa-md-4 mx-lg-auto" cols="6">
        <h2 class="pt-md-8 pb-md-8">Project</h2>
        <v-main>
          <router-view/>
        </v-main>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
