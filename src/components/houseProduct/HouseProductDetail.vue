<template>
  <v-container class="house-product-detail-main col-lg-7">
    <div class="detail-header">
      <div class="detail-road-view">
        <road-view-map></road-view-map>
      </div>
      <div class="detail-content">
        <div class="detail-content-apart-name">
          <p>{{ product.apartName }}</p>
        </div>

        <v-divider></v-divider>
        <!-- <div class="detail-content-layout">
          <p class="detail-content-title">매물 번호</p>
          <p class="detail-content-content">{{ product.houseProductId }}</p>
        </div> -->
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">주소</p>
          <p class="detail-content-content">
            {{ product.roadName }} {{ product.roadNameBonbun }} -
            {{ product.roadNameBubun }}
          </p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">건축 년도</p>
          <p class="detail-content-content">{{ product.buildYear }}</p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">층</p>
          <p class="detail-content-content">{{ product.floor }}</p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">면적</p>
          <p class="detail-content-content">{{ product.area }}</p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">거래유형</p>
          <p class="detail-content-content">{{ product.dealType | dealType }}</p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">가격</p>
          <p class="detail-content-content">
            {{ (parseInt(product.dealAmount.replace(",", "")) * 10000) | price }}원
          </p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">판매여부</p>
          <p class="detail-content-content">
            {{ product.stateFlag | stateFlag }}
          </p>
        </div>
        <v-divider></v-divider>
        <div class="detail-content-layout">
          <p class="detail-content-title">판매자</p>
          <p class="detail-content-content">{{ product.userId }}</p>
        </div>
        <v-divider></v-divider>
      </div>
    </div>

    <div class="detail-body-layout">
      <div class="detail-body-title">
        <p>💡 매물설명</p>
      </div>
      <v-divider></v-divider>
      <div v-html="product.content" class="detail-body-content">
        <!-- <p>{{ product.content }}</p> -->
      </div>
    </div>

    <div class="detail-body-layout">
      <div class="detail-body-title">
        <p>📌 매물 부가 조회</p>
      </div>
      <v-divider></v-divider>
      <div class="detail-body-content">
        <p>주변 상권</p>
        <p>★ 특징 ★</p>
        <p>✔ 6호선 구산역 도보 14분거리!</p>
        <p>✔ 불광천, 근린공원과 가까워 살기 좋습니다.</p>
        <p>✔ 카페, 편의점, 병원, 은행, 도서관, 마트, 주민센터 등 생활 인프라 좋습니다.</p>
      </div>
    </div>

    <div class="detail-body-layout">
      <div class="detail-body-title">
        <p>🔨 매물 안정성 검사</p>
      </div>
      <v-divider></v-divider>
      <div class="detail-body-content">
        <p>주변 상권</p>
      </div>
    </div>

    <v-divider></v-divider>

    <div class="detail-body-btns">
      <v-btn color="primary" @click="delProduct" v-if="userInfo.userId == product.userId">매물 삭제하기</v-btn>
      <!-- <v-btn color="primary">매물 수정하기</v-btn> -->
      <v-btn color="error">매물 신고하기</v-btn>
    </div>

    <v-divider></v-divider>
    <!-- <house-product-review :houseProductId="product.houseProductId"></house-product-review> -->
    <house-product-review v-if="userInfo != null"></house-product-review>
  </v-container>
</template>

<script>
import { mapState, mapActions } from "vuex";
import RoadViewMap from "../map/RoadViewMap.vue";
import HouseProductReview from "./HouseProductReview.vue";

const houseProductStore = "houseProductStore";
const memberStore = "memberStore";

export default {
  name: "HouseProductDetail",
  components: {
    RoadViewMap,
    HouseProductReview,
  },
  computed: {
    ...mapState(houseProductStore, ["product"]),
    ...mapState(memberStore, ["userInfo"]),
  },
  methods: {
    ...mapActions(houseProductStore, ["deleteProduct"]),
    delProduct() {
      this.deleteProduct(this.product.houseProductId);   // 매물 삭제 
      this.$router.push({ name: "houseProduct" }); 
    },
  },
  filters: {
    dealType(value) {
      if (value == "SALE") {
        return "매매";
      } else if (value == "YEAR") {
        return "전세";
      } else {
        return "월세";
      }
    },
    stateFlag(value) {
      if (value == 0) {
        return "판매중";
      }else {
        return "판매완료";
      }
    },
    price(value) {
      if (!value) return value;
      return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  data() {
    return {
      model: 0,
      colors: ["primary", "secondary", "yellow darken-2", "red", "orange"],
    };
  },
};
</script>

<style scoped>
.house-product-detail-main {
  background-color: white;
  padding: 50px;
  padding-top: 0px;
}

.detail-header {
  display: flex;
  justify-content: center;
  align-items: start;
}

.detail-content {
  width: 40%;
  margin-left: 50px;
}

.detail-content-apart-name {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 35px;
}

.detail-content-layout {
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

.detail-content-title {
  font-size: 0.9rem;
  font-weight: bold;
  width: 40%;
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.detail-content-content {
  width: 80%;
}

.v-application p {
  margin: 0;
}

.detail-body-layout {
  /* background-color: orange; */
  display: flex;
  flex-direction: column;
  margin-top: 50px;
}

.detail-body-title {
  font-size: 1.1rem;
  font-weight: bold;
  padding: 15px;
  padding-bottom: 10px;
}

.detail-body-content {
  /* background-color: teal; */
  padding: 15px;
  min-height: 150px;
}

.detail-body-btns {
  background-color: rgb(238, 238, 238);
  padding: 20px;
  display: flex;
  justify-content: end;
}

.detail-body-btns button {
  margin: 0px 10px;
}
</style>
