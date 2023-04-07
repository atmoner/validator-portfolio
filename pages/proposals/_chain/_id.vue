<template>
  <div>
  <v-card>
    test
  </v-card>
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
import cosmosConfig from '~/cosmos.config'

  export default {
    name: 'IndexPage',
    data () {
      return {
        cosmosConfig: cosmosConfig,
      }
    },
    computed: {
      ...mapState('data', ['accountInfo', 'isLogged']),
    },
    async mounted () {

    },
    methods: {
      openURL (url) {
        window.open(url)
      }
    }
  }
  </script>


