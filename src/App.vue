<template>
  <div id="app" class="container">

    <header class="p-3">
      <h2>HaveFun</h2>
      <i class="fas fa-search"></i>
      <input type="text" v-model="searchTerm" >
      <p v-if="searchResult">搜尋結果有{{searchResult.length}}筆</p>
    </header>
    <!-- 分頁 -->
    <div class="row">
      <el-pagination
        class="mx-auto m-5"
        background
        layout=" prev, pager, next ,total" 
        :total="searchResult.length" 
        :page-size="pageSize"
        :current-page.sync="currentPage"
        @current-change="handleCurrentChange">
      </el-pagination>
    </div>
    <div class="container">
      <div class="row">
        <ConditionFilter :zoneList="zoneList" 
        @searchZone="searchZone"
        @handleCharge="handleCharge"/>
        <ResultList :searchResult="covert_to_pageData" v-if="apiData_by_page"/>
        <ResultList :searchResult="apiData" v-else/>
        <!-- 分頁 -->
        <el-pagination
          class="mx-auto m-5"
          background
          layout=" prev, pager, next ,total" 
          :total="searchResult.length" 
          :page-size="pageSize"
          :current-page.sync="currentPage"
          @current-change="handleCurrentChange">
        </el-pagination>
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

      //初始化
      this.searchResult = this.apiData;
      //設定第一頁
      this.handleCurrentChange(1);
    });
  },
  data() {
    return {
      searchTerm: "",
      apiData: [],
      searchResult: [],
      zoneList: [],
      pageSize: 5,
      apiData_by_page: [],
      currentPage: 1
    };
  },
  computed: {
    covert_to_pageData() {
      this.apiData_by_page = this.searchResult;
      return this.apiData_by_page;
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
        this.searchResult = this.apiData.filter(item => {
          return item.Zone === zone;
        });
      } else {
        this.searchResult = this.apiData;
      }
    },
    handleCurrentChange(currentPage) {
      console.log(`当前页: ${currentPage}`);
      console.log(this.searchResult);

      // 用currenPage跟pageSize去filter searchResult
      this.searchResult = this.searchResult.filter((item, idx) => {
        //  抓出對應目前頁面的item
        //  (currentPage-1)*pageSize<= idx < currentPage*pageSize
        console.log(idx);
        if (
          idx >= (currentPage - 1) * this.pageSize &&
          idx < currentPage * this.pageSize
        ) {
          return item;
        }
      });
    }
  },
  watch: {
    searchTerm: function(newText, oldText) {
      // 若searchBar清空，則用原本資料顯示
      if (!newText) {
        this.searchResult = this.apiData;
      } else {
        // searchTerm有值，則過濾
        this.searchResult = this.apiData.filter(item => {
          return (
            item.Name.includes(newText) || item.Description.includes(newText)
          );
        });
        // 回page1
        this.handleCurrentChange(1);
        this.currentPage = 1;
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
