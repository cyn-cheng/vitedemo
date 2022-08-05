<template>
  <div id="topbar" class="grid-3"></div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps(["hightLight"]);

const name = ref("TopBar");
const classifications = ref([]);
const companyInfo = ref({});
</script>

<style lang="scss">
#topbar {
  a {
    line-height: 4rem;
  }
}
</style>

<style lang="scss" scoped>
#topbar {
  position: sticky;
  top: 0;
  width: 100%;
  padding: 0 10vw 0 10vw;
  height: 4rem;
  background: rgba(255, 255, 255, 0.5);
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(30px);
  box-sizing: border-box;
  z-index: 9000;

  #classification {
    display: inline-block;
    min-width: 2rem;
    margin: 0 0.75rem;
    transition: 200ms;
    &[class="hight-light"] {
      border-bottom: 2px solid grey;
    }
  }
  .company-name {
    font-size: 1.5rem;
    font-weight: bold;
    font-family: "楷体";
  }
  .company-icon {
    width: 120px;
    margin-right: 12px;
  }
}
</style>