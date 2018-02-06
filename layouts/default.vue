<template>
  <div>
    <HeaderBar @Open="openit"/>
    <nuxt/>
    <PopUp v-show="PopStatus" @Close="closeit"/>
  </div>
</template>

<script>
import HeaderBar from "~/components/HeaderBar.vue";
import PopUp from "~/components/PopUp.vue";

export default {
  data() {
    return {
      PopStatus: ""
    };
  },
  components: {
    HeaderBar,
    PopUp
  },
  methods: {
    openit() {
      this.PopStatus = true;
    },
    closeit() {
      this.PopStatus = false;
    }
  }
};
</script>

<style>

</style>
