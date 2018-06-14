<template>
  <div id="app" class="container">

    <header class="p-3">
      <h2>HaveFun</h2>
      <i class="fas fa-search"></i>
      <!-- 輸入搜尋字串 -->
      <!-- 有結果顯示幾筆，沒結果顯示無 -->
      <input type="text" v-model="searchTerm" >
      <p v-if="filterBySearch.length">搜尋結果有{{filterBySearch.length}}筆</p>
      <p v-else>無搜尋結果</p>
    </header>

    <div class="container">
      <div class="row">
        <!-- 左邊下搜尋條件 地址/地區/收費-->
        <!-- 由child comp上傳選擇event -->
        <ConditionFilter 
          :zoneList="zoneList" 
          @searchZone="searchZone"
          @handleCharge="handleCharge"/>
        <!-- 右邊顯示結果 -->
        <ResultList 
          :searchResult="filterBySearch" />

      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ConditionFilter from "./components/ConditionFilter";

import ResultList from "./components/ResultList";

const API =
  "https://data.kcg.gov.tw/api/action/datastore_search?resource_id=92290ee5-6e61-456f-80c0-249eae2fcc97&limit=88";

export default {
  name: "app",
  components: {
    ConditionFilter,
    ResultList
  },
  created() {
    // 抓API資料
    axios.get(API).then(res => {
      this.apiData = res.data.result.records;

      // 抓出所有行政區資料
      this.apiData.forEach((item, idx) => {
        // 如果zoneList內沒有此zone就加入array
        if (!this.zoneList.includes(item.Zone)) {
          this.zoneList.push(item.Zone);
        }
      });
    });
  },
  data() {
    return {
      searchTerm: "", //關鍵字search
      apiData: [], //api資料
      zoneList: [],
      filteredData: []
    };
  },
  filters: {},
  computed: {
    //回傳關鍵字搜尋的結果
    filterBySearch() {
      let searchTerm = this.searchTerm;
      let data = this.apiData;

      if (!searchTerm) {
        return data;
      } else {
        // searchTerm有值，則過濾比對每一筆的Name跟Description
        return data.filter(item => {
          return (
            item.Name.includes(searchTerm) ||
            item.Description.includes(searchTerm)
          );
        });
      }
    }
  },
  methods: {
    handleCharge(val) {
      console.log(val);
      // true是有收費
      if (val) {
        this.searchResult = this.apiData.filter(item => {
          return item.Ticketinfo !== "免費參觀";
        });
      } else {
      }
    },
    searchZone(zone) {
      if (zone) {
        this.filteredData = this.apiData.filter(item => {
          return item.Zone === zone;
        });
      } else {
        this.filteredData = this.apiData;
      }
    }
  }
};
</script>

<style scoped>
div {
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
}
header {
  display: flex;
  justify-content: flex-start;
}
.searchBar input {
  border: none;
  border-bottom: 3px solid #eee;
}
</style>
