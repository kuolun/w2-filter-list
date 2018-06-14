<template>
  <div class="ml-1 col-md-7">
    <!-- 分頁 -->
    <div class="row">
      <el-pagination
        class="mx-auto m-5"
        background
        layout=" prev, pager, next ,total" 
        :total="searchResult.length" 
        :page-size="pageSize"
        :current-page.sync="currPage"
        @current-change="handleChangePage">
      </el-pagination>
    </div>
    <ul class="pl-0">
      <ResultItem 
        :resultItem="item" 
        v-for="item in searchResult.slice(pageStart,pageStart+pageSize)" 
        :key="item.Id"/>
    </ul>
  </div>
</template>


<script>
import ResultItem from "./ResultItem";

export default {
  name: "ResultList",
  props: ["searchResult"], //傳入搜尋結果array
  data() {
    return {
      pageSize: 5,
      currPage: 1
    };
  },
  components: {
    ResultItem
  },
  computed: {
    pageStart: function() {
      return (this.currPage - 1) * this.pageSize;
    }
  },
  methods: {
    // 處理換頁
    handleChangePage(currPage) {
      console.log(`当前页: ${currPage}`);
      // 改變currPage去讓畫面rerender
      this.currPage = currPage;
    }
  },
  watch: {
    searchResult() {
      console.log("search changed");
      this.currPage = 1;
    }
  }
};
</script>


<style scoped>
div {
  border: 1px solid #eee;
  width: 760px;
}
</style>
