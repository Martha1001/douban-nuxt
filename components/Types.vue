<template>
  <div class="types f-ct">
    <h3>{{title}}</h3>
    <ul class="type-list f-cl">
      <!-- <li v-for="item in items">
        <a :href="baseUri + item.href" class="f-tof">{{item.title}}</a>
      </li> -->
      <li></li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["title", "items"],
  data() {
    return {
      baseUri: "https://m.douban.com/"
    };
  }
};
</script>

<style scoped>
.types h3 {
  margin-bottom: 15px;
  font-size: 16px;
}

.type-list li {
  position: relative;
  box-sizing: border-box;
  float: left;
  width: 49%;
  height: 40px;
  line-height: 40px;
  border-top: 1px solid #ddd;
}

.type-list li:nth-child(even) {
  padding-left: 15px;
}

.type-list li:nth-child(odd) {
  border-right: 1px solid #ddd;
}

.type-list li:last-child,
.type-list li:nth-last-child(2) {
  border-bottom: 1px solid #ddd;
}

.type-list a {
  color: #42bd56;
  font-size: 14px;
}

.type-list li:after {
  display: inline-block;
  content: "";
  position: absolute;
  right: 10px;
  top: 15px;
  width: 8px;
  height: 8px;
  border-bottom: 1px solid #ddd;
  border-right: 1px solid #ddd;
  transform: rotate(-45deg);
}
</style>