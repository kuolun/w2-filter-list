<template>
  <div class="col-md-4 fiter-section">
    <div class="keyword border-bottom mb-5">
      <h3>地址查詢</h3>
      <input type="text" v-model="address">
    </div>
    <div class="zone border-bottom mb-5">
<!--  篩選地區 -->
      <h3>地區</h3>

      <el-select 
          v-model="zone" 
          clearable 
          placeholder="请选择" 
          @change="searchZone">

        <el-option
          v-for="zone in zoneList"
          :key="zone"
          :label="zone"
          :value="zone">
        </el-option>

      </el-select>
    </div>
    <div class="fee border-bottom mb-5">
      <h3>收費</h3>
      <el-radio-group v-model="charge" @change="radioChange">
        <el-radio :label="0">All</el-radio>
        <el-radio :label="1">免費</el-radio>
        <el-radio :label="2">收費</el-radio>
      </el-radio-group>

    </div>
    <div class="opentime">
      <h3>開放時間</h3>
      <div class="block">
        <el-date-picker
          v-model="date"
          size="large"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期">
        </el-date-picker>
      </div>  
    </div>
  </div>
</template>


<script>
export default {
  name: "ConditionFilter",
  props: ["zoneList"],
  data() {
    return {
      address: "",
      zone: "",
      charge: 0,
      date: ""
    };
  },
  methods: {
    searchZone(zone) {
      this.$emit("searchZone", zone);
    },
    radioChange(val) {
      this.$emit("radioChange", val);
    }
  }
};
</script>


<style scoped>
.fiter-section {
  border: 1px solid #eee;
  width: 300px;
  height: 800px;
  background-color: #ebebeb;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>

