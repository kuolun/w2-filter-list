<template>
  <div class="ml-1 col-md-7">
    <ul class="pl-0">
        <ResultItem :resultItem="item" v-for="item in searchResult" :key="item.Id"/>
    </ul>
  </div>
</template>


<script>
import ResultItem from "./ResultItem";

export default {
  name: "ResultList",
  props: ["searchResult"],
  components: {
    ResultItem
  }
};
</script>


<style scoped>
div {
  border: 1px solid #eee;
  width: 760px;
}
</style>
