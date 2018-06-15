<template>
  <div id="app" class="container">

    <header class="p-3">
      <h2>HaveFun</h2>
      <i class="fas fa-search"></i>
      <!-- 輸入搜尋字串 -->
      <!-- 有結果顯示幾筆，沒結果顯示無 -->
      <input type="text" v-model="searchTerm" @input="searchKeyword">
      <p v-if="countRecords">搜尋結果有{{countRecords}}筆</p>
    </header>

    <div class="container">
      <div class="row">
        <!-- 左邊下搜尋條件 地址/地區/收費-->
        <!-- 由child comp上傳選擇event -->
        <ConditionFilter 
          :zoneList="zoneList" 
          @searchZone="searchZone"
          @radioChange="radioChange"/>
        <!-- 右邊顯示結果 -->
        <ResultList 
          :searchResult="filteredData ? filteredData : apiData" />

      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ConditionFilter from "./components/ConditionFilter";

import ResultList from "./components/ResultList";

const API =
  "https://data.kcg.gov.tw/api/action/datastore_search?resource_id=92290ee5-6e61-456f-80c0-249eae2fcc97&limit=200";

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
      this.filteredData = this.apiData;

      // 抓出所有行政區資料
      this.apiData.forEach((item, idx) => {
        // 如果zoneList內沒有此zone就加入array
        if (!this.zoneList.includes(item.Zone)) {
          this.zoneList.push(item.Zone);
        }
      });
      //設定為免費
      this.radioChange(0);
    });
  },
  data() {
    return {
      apiData: [], //api來源資料
      searchTerm: "", //關鍵字search
      addressTerm: "", //地址
      zoneList: [], //地區list
      selectedZone: "", //選擇地區
      filteredData: [], //過濾後資料
      charge: false, //收費與否
      dateTime: 0 //開放時間
    };
  },
  filters: {},
  computed: {
    countRecords() {
      if (this.searchTerm) {
        return this.filteredData.length;
      } else {
        return false;
      }
    }
  },
  methods: {
    //處理filter條件
    searchKeyword() {
      let searchTerm = this.searchTerm;
      // let selectedZone = this.selectedZone;
      // let charge = this.charge;
      let data = this.apiData;

      //如果有輸入input search
      if (searchTerm) {
        // searchTerm有值，則過濾比對每一筆的Name跟Description
        this.filteredData = data.filter(item => {
          return (
            item.Name.includes(searchTerm) ||
            item.Description.includes(searchTerm)
          );
        });
      }
    },
    radioChange(val) {
      console.log(val);
      // 0:all,1:free,2:charge
      // true是有收費
      if (val === 2) {
        console.log("收費");
        this.filteredData = this.apiData.filter(item => {
          return item.Ticketinfo !== "免費參觀" && item.Ticketinfo !== "";
        });
      } else {
        console.log("免費");
        this.filteredData = this.apiData.filter(item => {
          return item.Ticketinfo === "免費參觀" || item.Ticketinfo === "";
        });
        console.log(this.filteredData);
      }
    },
    // 選完地區後觸發
    searchZone(zone) {
      console.log(zone);
      if (zone) {
        // let test;
        this.filteredData = this.apiData.filter(item => {
          console.log(item.Zone);
          return item.Zone === zone;
        });
        // console.log(test);
        // this.filteredData = test;
      } else {
        //全部地區
        this.filteredData = null;
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
