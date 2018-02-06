<template>
  <div class="home-list">
    <ul>
      <!-- <li v-for="item in items">
        <router-link :to="{name:'Activity',params:{eventId:item.id}}">
          <div class="ct f-cl">
            <div class="img-wrap">
              <img :src="item.image" alt="">
            </div>
            <h3>{{item.title}}</h3>
            <p>{{item.content | subStr}}</p>
          </div>
          <div class="feed">
            <span class="author">by：{{item.category_name}}</span>
            <span class="from" v-if="item.subcategory_name">from：{{item.subcategory_name}}</span>
          </div>
        </router-link>
      </li> -->
    </ul>
  </div>

</template>
<script>
export default {
  props: {
    items: {
      type: Array,
      required: true
    }
  },
  filters: {
    subStr: function(val) {
      let newVal = val.replace(/<.*?>/g, "");
      return newVal.slice(0, 30);
    }
  }
};
</script>

<style scoped>
.home-list li {
  padding: 20px 0;
  border-bottom: 1px solid #eee;
}
.home-list .ct {
  margin-bottom: 8px;
}
.home-list .ct h3 {
  font-size: 16px;
  margin-bottom: 8px;
}
.home-list .ct p {
  color: #999;
  font-size: 12px;
}
.img-wrap {
  position: relative;
  float: right;
  width: 25%;
  height: 0;
  margin-left: 20px;
  padding-bottom: 25%;
}
.img-wrap img {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
}
.feed {
  color: #aaa;
  font-size: 12px;
}
.author,
.from {
  display: inline-block;
  width: 49%;
  max-width: 49%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.from {
  text-align: right;
}
</style>