<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="4"
      v-for="n in listValidators"
      :key="n.banner">
  <v-card
    class="mx-auto"
    max-width="600"

  >
    <v-img
      class="white--text align-end"
      height="200px"
      :src="n.banner"
    >
      <v-card-title>{{ n.chain.name}} | {{ n.description.moniker }}</v-card-title>
    </v-img>
<!--<pre>{{n}}</pre>-->

    <v-card-subtitle class="pb-0">
      {{n.valAddr}}
    </v-card-subtitle>

    <v-card-text class="text--primary">
      <v-simple-table>
        <template v-slot:default>
           <tbody>

            <tr>
              <td>Status</td>
              <td>
                <v-chip
                  v-if="n.status === 'BOND_STATUS_BONDED'"
                  color="#00b786"
                  outlined
                  label
                >
                  Online
                </v-chip>
              </td>

            </tr>

            <tr>
              <td>Tokens delegated</td>
              <td>{{ formatNum((n.tokens / 1000000).toFixed(2)) }}

                <v-btn
                  :color="n.chain.color"
                  text
                  tile
                >
                  {{ n.chain.coinLookup?.viewDenom }}
                </v-btn>
              </td>
            </tr>
            <tr>
              <td>Commission</td>
              <td>{{ n.commission.commission_rates.rate * 100 }}%</td>
            </tr>
            <tr>
              <td>Last update</td>
              <td>{{ n.commission.update_time | formatDate }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      <!--<div>Whitehaven Beach</div>

      <div>Whitsunday Island, Whitsunday Islands</div>-->
    </v-card-text>

    <v-card-actions>
    <v-spacer></v-spacer>

      <v-btn
        :color="n.chain.color"
        :to="'/proposals/' + n.chain.slug"
      >
        View votes
      </v-btn>
      <v-btn
        :color="n.chain.color"
        disabled
      >
        Delegate (soon)
      </v-btn>
    </v-card-actions>
  </v-card>
      <!--<v-item-group>
        <v-container>
          <v-row>
            <v-col
              v-for="n in cosmosConfig"
              :key="n"
              cols="12"
              md="6"
            >
              <v-item>
                <v-card

                  height="150"
                  width="500"
                >
                  {{n.name}}<br />
                  {{n.valAddr}}
                </v-card>
              </v-item>
            </v-col>
          </v-row>
        </v-container>
      </v-item-group>-->
    </v-col>
  </v-row>
</template>

<script>
import axios from 'axios'
import cosmosConfig from '~/cosmos.config'

export default {
  name: 'IndexPage',
  data: () => ({
    cosmosConfig: cosmosConfig,
    listValidators: []
  }),
  async mounted () {
    // console.log(this.logged)

    let finalValidators = []
    cosmosConfig.forEach(async (item) => {
      const getTotalDelegated = await axios(item.apiURL + '/cosmos/staking/v1beta1/validators/'+item.valAddr)
      console.log(getTotalDelegated.data)
      getTotalDelegated.data.validator.banner = item.coinLookup.banner
      getTotalDelegated.data.validator.chain = item
      finalValidators.push(getTotalDelegated.data.validator)
    });
    this.listValidators = finalValidators
  },
  methods: {
    formatNum(nombre){
      return new Intl.NumberFormat().format(nombre)
    },
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
