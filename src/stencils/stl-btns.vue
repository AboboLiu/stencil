<template>
  <div class="btns">
    <el-button v-for="(btn, i) in btns" :key="i" :icon="btn.icon" size="medium" @click="btnClick(btn.click)" :type="btn.type">{{btn.label}}</el-button>
  </div>
</template>
<script>
  export default {
    props: {
      btns: Object,
      pageModel: Object
    },
    methods: {
      btnClick(click) {
        click && click(this.pageModel);
      }
    }
  }

</script>
