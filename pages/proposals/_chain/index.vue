<template>
  <div>
  <v-card>
    <v-card-title>
      {{ pageNow }} Proposals  
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="proposals"
      :search="search"
    >
  
      <template #item.status="{ item }">
        <td v-if="item.status === 'PROPOSAL_STATUS_PASSED'">              
                <v-chip
                  class="ma-2"
                  color="green"
                  text-color="white"
                  label
                >
                  <v-icon class="mr-1">mdi-checkbox-marked-circle</v-icon>
                  Proposal Passed 
                </v-chip>              
              </td>
              <td v-if="item.status === 'PROPOSAL_STATUS_REJECTED'">
                <v-chip
                  class="ma-2"
                  color="red"
                  text-color="white"
                  label
                >
                  <v-icon class="mr-1">mdi-delete-forever</v-icon>
                  Proposal Rejected
                </v-chip>                  
              </td>
              <td v-if="item.status === 'PROPOSAL_STATUS_VOTING_PERIOD'">
                <!--{{ item.status }}-->
                <v-chip
                  class="ma-2"
                  text-color="white"
                  color="primary"
                  label
                >
                  <v-icon class="mr-1">
                    mdi-alarm-check
                  </v-icon>
                  Voting Period
                </v-chip>              
              </td>
              <td v-if="item.status === 'PROPOSAL_STATUS_DEPOSIT_PERIOD'">
                <!--{{ item.status }}-->
                <v-chip
                  class="ma-2"
                  text-color="white"
                  color="primary"
                  label
                >
                  <v-icon class="mr-1">
                    mdi-cash-fast
                  </v-icon>
                  Deposit Period
                </v-chip>              
              </td>        
      </template>  
      <template #item.voting_start_time="{ item }">
        {{ item.voting_start_time | formatDate }}     
      </template>        
      <template #item.voting_end_time="{ item }">
        {{ item.voting_end_time | formatDate }}     
      </template>      
      <template #item.myvote="{ item }">
        <v-btn
          class="ma-2"  
          disabled
        >
          View my vote (soon)
        </v-btn>
      </template>            
    </v-data-table>
  </v-card>
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
import axios from 'axios'
import cosmosConfig from '~/cosmos.config'

  export default {
    name: 'IndexPage',
    
    data () {
      return {
        cosmosConfig: cosmosConfig,
        pageNow: '',
        proposals: [],
        search: '',
        findChain: {},
        headers: [
          { text: 'Id', value: 'proposal_id' },
          {
            text: 'Title',
            align: 'start',
            sortable: false,
            value: 'content.title',
          },
          { text: 'Status', value: 'status' },
          { text: 'Start time', value: 'voting_start_time' },
          { text: 'End time', value: 'voting_end_time' },
          { text: 'My vote', value: 'myvote' },
        ],        
      }
    },
    watch: {

    },
    computed: {
      ...mapState('data', ['accountInfo', 'isLogged']),
    },
    async mounted () {
      this.pageNow = this.$nuxt.$route.params.chain
      let findChain = this.cosmosConfig.find(chain => chain.slug === this.$nuxt.$route.params.chain)
      this.findChain = findChain
      console.log(findChain)
      // const allProposals = await axios(findChain.apiURL + `/cosmos/gov/v1beta1/proposals`)

      try {
        const allProposals = await axios(findChain.apiURL + `/cosmos/gov/v1beta1/proposals`)
        console.log(allProposals)
        let setFinalPropos = []
        allProposals.data.proposals.forEach((item) => {
          setFinalPropos.push(item);
        });
        this.proposals = setFinalPropos.reverse()          
      } catch (error) {
        console.log(error)
      }


      

    },
    methods: {
      openURL (url) {
        window.open(url)
      }
    },
    filters: {
      formatDate: (dateStr) =>
        Intl.DateTimeFormat("us-EN",
          {
            year: "numeric",
            month: "numeric",
            day: "numeric",
            hour: "numeric",
            minute: "numeric",
            second: "numeric",
            hour12: false
          }).format(new Date(dateStr)),
    }    
  }
  </script>


