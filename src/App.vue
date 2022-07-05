<template>
  <div class="aa">
    <h1>图书管理系统</h1>
    <div class="bb">
      <input
        type="text"
        v-model.trim="input"
        placeholder="搜索-书本名称"
        class="cc"
        @keydown.enter="enter"
      />
      <div>
        <el-button type="text" @click="centerDialogVisible = true"
          >添加图书</el-button
        >
        <el-dialog
          title="提示"
          :visible.sync="centerDialogVisible"
          width="30%"
          center
        >
          <!-- <span>需要注意的是内容是默认不居中的</span> -->
          <input
            type="text"
            placeholder="书名"
            style="
              width: 20;
              height: 20px;
              margin: 15px;
              border-left: 1px solid;
            "
            v-model.trim="bookObj.bookname"
          />
          <input
            type="text"
            placeholder="作者"
            style="width: 20; height: 20px; margin: 15px"
            v-model.trim="bookObj.author"
          />
          <input
            type="text"
            placeholder="出版社"
            style="width: 20; height: 20px; margin: 15px"
            v-model.trim="bookObj.publisher"
          />
          <span slot="footer" class="dialog-footer">
            <el-button @click="centerDialogVisible = false">取 消</el-button>
            <el-button
              type="primary"
              @click="centerDialogVisible = false"
              style="display: none"
              >确 定</el-button
            >
            <el-button type="primary" @click="addFn">添加</el-button>
          </span>
        </el-dialog>
      </div>
    </div>
    <el-table :data="tableData" border style="width: 1000px">
      <el-table-column fixed prop="id" label="序号" width="200">
      </el-table-column>
      <el-table-column prop="bookname" label="书名" width="200">
      </el-table-column>
      <el-table-column prop="author" label="作者" width="200">
      </el-table-column>
      <el-table-column prop="publisher" label="出版社" width="200">
      </el-table-column>
      <el-table-column prop="address" label="操作" width="%20">
        <template slot-scope="scope"
          ><el-button
            type="danger"
            icon="el-icon-delete"
            style="margin-right: 5px"
            circle
            @click="del(scope.row.id)"
          ></el-button>
          <el-popover
            placement="top-start"
            title="详情"
            width="200"
            trigger="hover"
            :content="`书名:${scope.row.bookname} -
              作者:${scope.row.author} -
              出版社:${scope.row.publisher}`"
          >
            <el-button
              slot="reference"
              type="primary"
              icon="el-icon-share"
              circle
            ></el-button>
          </el-popover>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  methods: {
    handleClick(row) {
      console.log(row);
    },
  },

  data() {
    return {
      input: '',
      tableData: [],
      centerDialogVisible: false,
      bookObj: {
        bookname: '',
        author: '',
        publisher: '',
      },
    };
  },
  created() {
    this.$axios({
      url: '/api/getbooks',
    }).then((res) => {
      // console.log(res.data.data);
      this.tableData = res.data.data;
    });
  },
  methods: {
    // 搜索
    enter() {
      if (this.input.length === 0) {
        this.$axios({
          url: '/api/getbooks',
        }).then((res) => {
          // console.log(res.data.data);
          this.tableData = res.data.data;
        });
      }
      this.$axios({
        url: '/api/getbooks',
        params: {
          bookname: this.input,
        },
      }).then((res) => {
        // console.log(res.data.data);
        this.tableData = res.data.data;
      });
      this.input = '';
    },
    //删除
    del(id) {
      // this.tableData = this.tableData.filter(ele => ele.id != id)
      this.$axios({
        url: '/api/delbook',
        params: {
          id: id,
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
        },
      }).then((res) => {
        // console.log(res);
        this.$axios({
          url: '/api/getbooks',
        }).then((res) => {
          // console.log(res.data.data);
          this.tableData = res.data.data;
        });
      });
    },
    //添加
    addFn() {
      if (this.bookObj.bookname == '' || this.bookObj.author == '' || this.bookObj.publisher == '') {
        return alert('Please select a bookname');
      }
      this.$axios({
        url: '/api/addbook',
        method: 'POST',
        data: {
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
          ...this.bookObj,
        },
      });
      alert('添加成功');
      this.centerDialogVisible = false;
      this.bookObj.bookname = '';
      this.bookObj.author = '';
      this.bookObj.publisher = '';
    },
  },
};
</script>
<style scoped>
.aa {
  margin-top: 100px;
  margin-left: 400px;
}
.bb {
  display: flex;
  justify-content: space-between;
  width: 1000px;
  margin-bottom: 20px;
}
.cc {
  width: 300px;
}
</style>
