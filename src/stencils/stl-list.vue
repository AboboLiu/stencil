<template>
  <el-container v-loading="!data">
    <el-header height="auto">
      <stl-search :form="config.search" inline>
        <el-form-item label=" " slot="after">
          <el-button type="success" size="mini" @click="getData">搜索</el-button>
          <el-button size="mini" @click="clear">重置</el-button>
        </el-form-item>
      </stl-search>
    </el-header>
    <el-main>
      <stl-btns :btns="config.btns" :pageModel="pageModel"></stl-btns>
      <el-table :data="data" ref="table">
        <el-table-column type="index" label=" " width="40" align="center" header-align="center"></el-table-column>
        <el-table-column type="selection" width="35" align="center" header-align="center"></el-table-column>
        <el-table-column v-for="col in cols" :label="col.label" :prop="col.prop" :key="col.key" :align="col.align||'center'" :width="col.width" header-align="center" :formatter="col.formatter"></el-table-column>
        <el-table-column label="操作" width="120" header-align="center">
          <span slot-scope="scope">
            <el-button @click="toView(scope)" type="text" size="small">查看</el-button>
            <el-button @click="toEdit(scope)" type="text" size="small">编辑</el-button>
            <el-button @click="del(scope)" type="text" size="small">删除</el-button>
            <el-button v-for="btn in config.listBtns" type="text" size="small" v-text="btn.label" @click="btn.click"></el-button>
          </span>
        </el-table-column>
      </el-table>
      <el-pagination @size-change="getData" @current-change="getData" :current-page="page" :page-sizes="[100, 200, 300, 400]" :page-size="100" layout="total, sizes ,jumper ,-> , prev,pager, next" :total="400" background>
      </el-pagination>
    </el-main>
  </el-container>
</template>
<script>
  import '../style/list.scss';
  import stlSearch from './stl-form.vue';
  import stlBtns from './stl-btns.vue';
  export default {
    props: {
      config: Object
    },
    components: {
      stlSearch,
      stlBtns
    },
    data() {
      return {
        data: null,
        cols: [],
        page: 1,
        pageModel: null
      };
    },
    created() {
      this.setTitle();
      this.setCols();
      this.getData();
    },
    mounted() {
      let pageModel = {
        page: this.$refs.table,
        data: this.data
      };
      this.$set(this, 'pageModel', pageModel);
    },
    methods: {
      setTitle() {
        if (this.config.title) {
          window.document.title = this.config.title;
        }
      },
      setCols() {
        if (this.config.cols) {
          this.cols = this.config.cols;
        }
      },
      getData() {
        if (this.config.data) {
          this.data = this.config.data;
        } else {
          this.data = [];
        }
      },
      getPath(type) {
        let p = this.$route.path;
        return p.substring(0, p.lastIndexOf('/') + 1) + 'info/' + type;
      },
      toView(e) {
        this.$router.push(this.getPath('view'));
      },
      toEdit(e) {
        this.$router.push(this.getPath(e.row.id));
      },
      clear() {
        for (let key in this.config.search) {
          this.$set(this.config.search[key], 'value', '');
        }
      },
      del(e) {
        this.$confirm('确定删除吗？', '确认', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.data.splice(e.$index, 1);
          this.getData();
        }).catch(() => {
          // --
        });
      }
    }
  }

</script>
