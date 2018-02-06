<template>
  <div class="quick-nav">
    <ul class="f-cl">
      <li>
        <router-link to="">影院热映</router-link>
      </li>
      <li>
        <router-link to="">变量</router-link>
      </li>
      <li>
        <router-link to="">豆瓣时间</router-link>
      </li>
      <li>
        <router-link to="">使用豆瓣APP</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
</script>

<style scoped>
.quick-nav {
  padding-top: 24px;
}

.quick-nav li {
  box-sizing: border-box;
  float: left;
  width: 50%;
  padding: 3px;
}

.quick-nav li a {
  display: block;
  line-height: 40px;
  font-size: 14px;
  text-align: center;
  background: #eee;
  max-width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>