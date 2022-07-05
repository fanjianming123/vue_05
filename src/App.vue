<template>
  <div>
    <el-input
      v-model.trim="find"
      placeholder="搜索-书名名称"
      style="width: 180px"
    ></el-input>
    <button @click="findFn">搜索</button>
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column
        fixed
        prop="id"
        label="序号"
        width="300"
        v-model="objBook.id"
      ></el-table-column>
      <el-table-column
        prop="bookname"
        label="书名"
        width="300"
        v-model="objBook.bookname"
      >
      </el-table-column>
      <el-table-column
        prop="author"
        label="作者"
        width="200"
        v-model="objBook.author"
      ></el-table-column>
      <el-table-column
        prop="publisher"
        label="出版商"
        width="300"
        v-model="objBook.publisher"
      >
      </el-table-column>
      <el-table-column label="操作" width="300">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small"
            >删除</el-button
          >
          <el-button type="text" size="small">详情</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      msg:'获取图书列表成功',
      objBook: {
        id:'',
        bookname: '',
        author: '',
        publisher: '',
      },
      id:'',
      find: '',
      bookname: '',
      author: '',
      publisher: '',
    }
  },
  methods: {
    handleClick(row) {
      console.log(row)
    },
    findFn() {
      if(this.find.length==0) return
      console.log(this.find);
      this.$axios({
          url: 'http://www.liulongbin.top:3006/api/getbooks',
          method: 'GET',
          params: {
            bookname: this.find,
          },
        }).then((res)=>{
          console.log(res.data.data);
          this.tableData = res.data.data
          // console.log(this.tableData);
        })
      // if (this.find != '') {
      //   this.$axios({
      //     url: 'http://www.liulongbin.top:3006/api/getbooks',
      //     method: 'GET',
      //     params: {
      //       bookname: this.find,
      //     },
      //   }).then((res)=>{
      //     console.log(res.data.data);
      //     this.tableData = res.data.data
      //     console.log(this.tableData);
      //   })
      // }else {
      //   this.$axios({
      //     url: "/api/getbooks",
      //   }).then((res) => {
      //     console.log(res.data.data);
      //     this.tableData = res.data.data;
      //     console.log(this.tableData);
      //   });
      // }
    },
  },
  created() {
    this.$axios({
      url: 'http://www.liulongbin.top:3006/api/getbooks',
      method: 'GET', // 默认就是GET方式请求, 可以省略不写
    }).then((res) => {
      // console.log(res.data.data)
      this.tableData = res.data.data
    })
    // axios()-原地得到Promise对象
  },
  
}
</script>
