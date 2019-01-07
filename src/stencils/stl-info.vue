<template>
  <el-main>
    <stl-form :form="config.form" :styleMap="styleMap"></stl-form>
    <stl-btns :btns="config.btns"></stl-btns>
  </el-main>
</template>
<script>
  import '../style/info.scss';
  import stlForm from './stl-form';
  import stlBtns from './stl-btns';

  export default {
    components: {
      stlForm,
      stlBtns
    },
    props: {
      config: Object
    },
    created() {
      this.config.btns.save = {
        label: '保存',
        type: 'primary',
        click: () => {
          this.submit(this.goBack);
        }
      };
      this.config.btns.saveNext = {
        label: '保存并继续',
        type: 'primary',
        click: () => {
          this.submit(this.clear);
        }
      };
      this.config.btns.goBacl = {
        label: '取消',
        click: () => {
          this.goBack();
        }
      };
    },
    data() {
      return {
        styleMap: {
          width: '500px'
        }
      };
    },
    methods: {
      submit(callback) {
        let datas = {};
        for (let key in this.config.form) {
          datas[key] = this.config.form[key].value;
        }
        callback && callback();
      },
      goBack() {
        this.$router.go(-1);
      },
      clear() {
        for (let key in this.config.form) {
          this.config.form[key].value = '';
        }
      }
    }
  }

</script>
