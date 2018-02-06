<template>
  <ul class="tags f-cl">
    <h3 class="title">{{title}}</h3>
    <!-- <li v-for="item in items">
      <router-link v-if="!item.title" to="/">{{item}}</router-link>
      <router-link v-else to="/">{{item.title}}</router-link>
    </li> -->
  </ul>
</template>

<script>
export default {
  props: ["title", "items"]
};
</script>

<style scoped>
.tags {
  margin-bottom: 15px;
}

.title {
  margin-bottom: 10px;
  font-size: 16px;
}

.tags li {
  float: left;
  margin: 0 10px 10px 0;
  padding: 3px 15px;
  color: #666;
  font-size: 13px;
  background: #eee;
  border-radius: 10px;
}
</style>