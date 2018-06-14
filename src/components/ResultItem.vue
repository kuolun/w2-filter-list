<template>
    <li>
      <div>
        <img :src="resultItem.Picture1" >
      </div>
      <div>
        <h4>{{resultItem.Name}}</h4>
        <p>{{resultItem.Description.slice(0,62)}}</p>
        <span><i class="fas fa-map-marker-alt"></i>{{resultItem.Zone}}</span>
        <span class="ml-5"><i class="fas fa-map-signs"></i>{{resultItem.Add}}</span>
        <p class="mt-2"><i class="far fa-calendar-alt"></i>{{resultItem.Opentime}}</p>
        <p><i class="fas fa-dollar-sign"></i>{{(resultItem.Ticketinfo==="")?"免費參觀":resultItem.Ticketinfo}}</p>
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
  height: 60px;
  overflow: hidden;
  margin: 0;
}
</style>

