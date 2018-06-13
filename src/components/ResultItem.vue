<template>
    <li>
      <img :src="resultItem.Picture1" >
      <div>
        <h4>{{resultItem.Name}}</h4>
        <p>{{resultItem.Description}}</p>
        <span><i class="fas fa-map-marker-alt"></i>{{resultItem.Zone}}</span>
        <span class="ml-5"><i class="fas fa-map-signs"></i>{{resultItem.Add}}</span>
        <p><i class="far fa-calendar-alt"></i>{{resultItem.Opentime}}</p>
      </div>

  </li>
</template>


<script>
export default {
  name: "ResultItem",
  props: ["resultItem"]
};
</script>

<style scoped>
li {
  border: 1px solid #eee;
  display: flex;
}
li img {
  width: 220px;
  height: 220px;
}
h4 {
  color: #9013fe;
  font-weight: bold;
}
p {
  width: 520px;
  height: 50px;
  overflow: hidden;
}
</style>

