<template>
  <div class="row justify-center q-pt-xl">
    <div class="col-md-12 col-lg-6 col-xs-12 q-pt-xl">
      <div>
        <q-splitter v-model="splitterModel" style="height: 450px">
          <template v-slot:before>
            <q-tabs v-model="tab" vertical class="text-purple-9">
              <q-tab
                name="mails"
                icon="refresh"
                label="reload"
                @click.prevent="getQuotes()"
              />
            </q-tabs>
          </template>

          <template v-slot:after>
            <q-tab-panels
              v-model="tab"
              animated
              swipeable
              vertical
              transition-prev="jump-up"
              transition-next="jump-up"
            >
              <q-tab-panel name="mails">
                <div v-for="dt in data" :key="dt.id">
                  <transition
                    appear
                    enter-active-class="animated fadeIn"
                    leave-active-class="animated fadeOut"
                  >
                    <div v-if="showSimulatedReturnData">
                      <div v-if="!onGetAuthorInfo" class="text-h5 q-mb-md">
                        {{ dt.quoteAuthor }}
                      </div>
                      <div class="text-weight-light q-pb-lg">
                        {{ dt.quoteText }}
                      </div>
                      <q-separator v-if="onGetAuthorInfo" class="q-mb-lg" />
                      <div class="q-mt-xl" v-if="!onGetAuthorInfo">
                        <q-btn
                          class="full-width"
                          color="purple-10"
                          icon-right="send"
                          label="more quotes"
                          @click.prevent="getAuthorQuotes(dt.quoteAuthor)"
                        />
                      </div>
                    </div>
                  </transition>
                </div>
              </q-tab-panel>
            </q-tab-panels>
          </template>
        </q-splitter>
      </div>
      <div>
        <q-inner-loading :showing="visible">
          <q-spinner-dots size="60px" color="purple-9" />
        </q-inner-loading>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      data: [],
      tab: "mails",
      splitterModel: 20,
      visible: false,
      showSimulatedReturnData: false,
      onGetAuthorInfo: false,
    };
  },
  methods: {
    async getQuotes() {
      this.visible = true;
      this.showSimulatedReturnData = false;

      await axios
        .get("https://quote-garden.herokuapp.com/api/v2/quotes/random")
        .then((res) => {
          this.data = [];
          this.data.push({
            _id: res.data.quote._id,
            quoteText: res.data.quote.quoteText,
            quoteAuthor: res.data.quote.quoteAuthor,
            quoteGenre: res.data.quote.quoteGenre,
          });
        })
        .catch((err) => console.log(err))
        .finally(() => {
          this.visible = false;
          this.showSimulatedReturnData = true;
          this.onGetAuthorInfo = false;
        });
    },
    async getAuthorQuotes(value) {
      await axios
        .get(
          `https://quote-garden.herokuapp.com/api/v2/authors/${value}?page=1&limit=10`
        )
        .then((res) => {
          this.data = [];
          if (res.data.quotes.length) {
            res.data.quotes.forEach((e) => {
              this.data.push({
                _id: e._id,
                quoteText: e.quoteText,
                quoteAuthor: e.quoteAuthor,
                quoteGenre: e.quoteGenre,
              });
            });
          }
          this.onGetAuthorInfo = true;
        });
    },
  },
  async created() {
    await this.getQuotes();
  },
};
</script>

<style></style>
