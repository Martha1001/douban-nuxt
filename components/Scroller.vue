<template>
  <div class="scroller">
    scroller
    <!-- <div class="img-box" v-if="type == 'imgList'">
      <h3 class="f-ct">
        <span>{{title}}</span>
        <router-link to="" class="more">更多</router-link>
      </h3>
      <ul class="img-list">
        <li v-for="item in items">
          <router-link :to="'subject/' + item.id" append>
            <div class="img-wrap"><img :src="item.images.large" alt=""></div>
            <p class="f-tof">{{item.title}}</p>
            <rating :rating="item.rating"></rating>
          </router-link>
        </li>
      </ul>
    </div>
    <div class="find-box" v-if="type == 'findList'">
      <h3 class="f-ct">{{title}}</h3>
      <ul class="find-list f-cl">
        <li v-for="item in items" v-if="!item.line">
          <a :href="item.href" :style="{color:item.color,borderColor:item.color}">{{item.title}}</a>
        </li>
        <li v-else class="line"></li>
      </ul>
    </div> -->
  </div>
</template>

<script>
export default {
  // props: ["title", "type", "items"],
  components: {

  }
};
</script>

<style scoped>
.scroller {
  margin: 10px 0 20px;
}

.img-box h3,
.find-box h3 {
  display: flex;
  margin-bottom: 15px;
  font-size: 16px;
  justify-content: space-between;
}

.img-box h3 .more {
  color: #42bd56;
  font-size: 14px;
}

.img-list {
  white-space: nowrap;
  overflow-x: auto;
}

.img-list li {
  display: inline-block;
  width: 100px;
  height: 188px;
  margin-right: 10px;
  text-align: center;
  vertical-align: top;
}

.img-list li:first-child {
  margin-left: 18px;
}

.img-list li:last-child {
  margin-right: 18px;
}

.img-list li .img-wrap {
  position: relative;
  width: 100%;
  height: 0;
  padding-top: 142%;
}

.img-list li img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.img-list li p {
  margin: 5px 0;
  font-size: 14px;
}

.find-box {
  margin-bottom: -10px;
}

.find-list {
  padding: 0 8px;
  white-space: nowrap;
  overflow-x: auto;
}

.find-list li {
  display: inline-block;
}

.find-list li a {
  display: block;
  margin: 0 0px 10px 10px;
  padding: 12px 25px;
  font-size: 14px;
  border-width: 1px;
  border-style: solid;
  border-radius: 3px;
}
.find-list .line {
  display: block;
  width: 100%;
  height: 1px;
}
</style>