<template>
  <v-col cols="4">
    <v-hover>
      <template v-slot:default="{ hover }">
        <v-card class="ma-4">
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/forest-art.jpg"
          ></v-img>
          <!-- <v-card-text>
            <h2 class="text-h6 primary--text">{{ report.subject }}</h2>
            {{ report.content }}
          </v-card-text> -->
          <v-card-text>
            <h2 class="text-h6 primary--text">{{ report.subject }}</h2>
            <!-- <h4 v-html="report.content"></h4> -->
          </v-card-text>
          <v-fade-transition>
            <v-overlay v-if="hover" absolute color="#3876f2">
              <v-btn @click="moveDetailPage()">더보기</v-btn>
            </v-overlay>
          </v-fade-transition>
        </v-card>
      </template>
    </v-hover>
  </v-col>
</template>

<script>
import { mapActions } from "vuex";

const boardStore = "boardStore";

export default {
  name: "BoardReportItem",
  props: {
    report: Object,
  },
  data() {
    return {};
  },
  methods: {
    ...mapActions(boardStore, ["getDetailReportArticle"]),
    moveDetailPage() {
      this.getDetailReportArticle(this.report);
      this.$router.push({ name: "BoardReportDetail" });
    },
  },
  filters: {
  },
};
</script>

<style></style>
