<template>
  <div>
    <div class="mb-lg">
      <b-row>
        <b-col xxl="3" xl="4" lg="6" md="12" sm="12" xs="12" class="py-1 py-md-2" v-for="(token, index) in visibleTokens" :key="index">
          <div class="pb-xlg" @click="$router.push('/app/token/' + token.symbol)">
            <Widget class="mb-0 token-card">
              <b-row class="token-details">
                <b-col cols="2" class="token-logo">
                  <img v-if="token.logoURI" :src="token.logoURI" loading="lazy">
                  <span class="no-token-emoji" v-else>🤔</span>
                </b-col>
                 <b-col 
                  cols="5" 
                  sm="4" 
                  md="6"
                  class="h4 token-title pr-0"
                  v-if="token.tags.includes('custom-urls')"
                >
                
                  {{ token.symbol | maxLenght(15) }}
                  <br>
                  <div class="token-name">
                  {{ dataFeeds["redstone-custom-urls-demo"].currentManifest.tokens[token.symbol].comment }}
                  </div>
                </b-col>               
                <b-col 
                  cols="5" 
                  sm="4" 
                  md="6"
                  class="h4 token-title pr-0"
                  v-else
                >
                  {{ token.symbol | maxLenght(8) }}
                  <br>
                  <div class="token-name">
                  {{ token.name }}
                  </div>
                </b-col>
                <b-col
                  cols="5"
                  sm="6"
                  md="4"
                  class="token-price pl-0">
                  <span v-if="!token.tags.includes('custom-urls')">
                    <span v-if="prices[token.symbol]">
                      {{ prices[token.symbol] | price({ eNotationForSmallValues: true }) }}
                    </span>
                    <vue-loaders-ball-beat
                      v-else
                      color="var(--redstone-red-color)"
                      scale="0.5"
                    ></vue-loaders-ball-beat>
                  </span>
                </b-col>
              </b-row>
              <b-row class="d-flex justify-content-end see-more-wrapper">
                <div class="see-more">See more<i class="fa fa-angle-right" /></div>  
              </b-row>
            </Widget>
          </div>
        </b-col>
      </b-row>
      <b-row>
        <div v-if="!allTokensVisible" v-observe-visibility="loadMoreSectionVisibilityChanged" class="loading-more-container">
          <vue-loaders-ball-beat
            color="var(--redstone-red-color)"
            scale="0.5"
          ></vue-loaders-ball-beat>
        </div>
      </b-row>
    </div>
  </div>
</template>

<script>
import Widget from "@/components/Widget/Widget";
import _ from "lodash";
import { mapState } from "vuex";
import showMoreTokensMixin from '@/mixins/show-more-tokens';

export default {
  name: 'Tokens',

  mixins: [showMoreTokensMixin],

  props: {
    tokens: Array,
  },

  data() {
    return {
    };
  },

  methods: {
    providerLabel(provider) {
      return _.startCase(provider)
    },

    loadMoreSectionVisibilityChanged() {
      this.showMoreTokens();
    },
  },

  computed: {
    ...mapState("prices", ["prices"]),
    ...mapState("prefetch", {
      dataFeeds: (state) => state.providers
    })
  },

  components: {
    Widget,
  },
}
</script>

<style lang="scss" src="./Tokens.scss" scoped />
