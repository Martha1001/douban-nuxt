<template>
  <div class="banner">
    <img class="bg" src="~static/promotion_bg.jpg" alt="">
    <!-- <div class="ct f-cl">
      <div>
        <p class="tit">{{title}}</p>
        <div class="btn">
          <span class="active">极速下载</span>
          <span>打开</span>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  // props: ["title"]
};
</script>

<style scoped>
.ad-banner {
  position: relative;
  height: 0;
  padding-top: 21.33%;
}

.bg,
.ct {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.ct > div {
  display: flex;
  position: absolute;
  top: 0;
  left: 20px;
  right: 10px;
  height: 100%;
  color: #2ca532;
  font-size: 14px;
  align-items: center;
  justify-content: space-between;
}

.tit {
  display: inline-block;
  vertical-align: middle;
}

.btn {
  display: inline-block;
  font-size: 12px;
}
.btn span {
  margin: 0 2px;
  padding: 5px 14px;
  font-weight: bold;
  border: 1px solid #2ca532;
  border-radius: 2px;
}
.btn span.active {
  color: #fff;
  background: #2ca532;
}
</style>