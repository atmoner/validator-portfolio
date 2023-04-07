
<template>
  <v-app id="inspire">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">

        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="32"
        >
          <img
            src="https://pbs.twimg.com/profile_images/1600017963435925504/khyXo-uF_400x400.jpg"
            alt="John"
          >
        </v-avatar>

        <v-btn
          text
          to="/"
        >
          Dashboard
        </v-btn>
        <v-btn
          text
        >
          Contact
        </v-btn>        
        <v-spacer></v-spacer>

      </v-container>
    </v-app-bar>

    <v-main class="grey lighten-3">
  <v-parallax
    height="300"
    dark
    src="https://cdn.pixabay.com/photo/2020/08/01/19/09/banner-5456101_960_720.png"
  >
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        class="text-center"
        cols="12"
      >
        <h1 class="text-h4 font-weight-thin mb-4">
          Leonoor's Cryptoman
        </h1>
        <h4 class="subheading">
          Your validator in cosmos ecosystem
        </h4>
      </v-col>
    </v-row>
  </v-parallax>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      links: [
        'Dashboard',
        'Contact'
      ],
    }),
  }
</script>
