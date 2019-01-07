<template>
  <el-form :model="form" :inline="inline" label-position="right" label-width="90px" size="mini" :style="styleMap">
    <template v-for="item in form">
      <h6 v-if="item.type==='tag-title'" v-text="item.label" class="tag"></h6>
      <el-form-item v-else-if="item.type === 'switch'" :label="item.label">
        <el-switch v-model="item.value"></el-switch>
      </el-form-item>
      <el-form-item v-else :label="item.label">
        <el-input v-model="item.value"></el-input>
      </el-form-item>
    </template>
    <slot name="after"></slot>
  </el-form>
</template>
<script>
  import '../style/form.scss';
  export default {
    props: {
      form: Object,
      inline: Boolean,
      styleMap: Object
    },
    data() {
      return {}
    },
    created() {
      for (let key in this.form) {
        this.$set(this.form[key], 'value', this.form[key].value || '');
      }
    }
  }

</script>
