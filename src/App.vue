<template>
  <div>
    <!-- header -->
    <el-input
      v-model.trim="find"
      placeholder="搜索-书名名称"
      style="width: 180px"
    ></el-input>
    <!-- body -->
    <button @click="findFn">搜索</button>
    <el-table
      :data="tableData"
      @selection-change="handleSelectionChange"
      border
      style="width: 100%"
    >
      <el-table-column
        fixed
        prop="id"
        label="序号"
        width="300"
      ></el-table-column>
      <el-table-column prop="bookname" label="书名" width="300">
      </el-table-column>
      <el-table-column prop="author" label="作者" width="200"></el-table-column>
      <el-table-column prop="publisher" label="出版商" width="300">
      </el-table-column>
           <el-table-column prop="address" label="操作" width="%20">
        <!-- 删除        -->
        <template slot-scope="scope"
          ><el-button
            type="danger"
            icon="el-icon-delete"
            style="margin-right: 5px"
            circle
            @click="delFn(scope.row.id)"
          ></el-button>
                   
          <el-popover
            placement="top-start"
            title="详情"
            width="200"
            trigger="hover"
            :content="`书名:${scope.row.bookname}-作者:${scope.row.author}-出版社:${scope.row.publisher}`"
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

    <!-- footer -->
    <div style="border-top: 1px solid">
      <input
        type="text"
        placeholder="书名"
        style="width: 20; height: 20px; margin: 15px; border-left: 1px solid"
        v-model="objBook.bookname"
      />
      <input
        type="text"
        placeholder="作者"
        style="width: 20; height: 20px; margin: 15px"
        v-model="objBook.author"
      />
      <input
        type="text"
        placeholder="出版社"
        style="width: 20; height: 20px; margin: 15px"
        v-model="objBook.publisher"
      />
      <el-button type="primary" round @click="addFn">添加</el-button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      msg: '获取图书列表成功',
      objBook: {
        id: '',
        bookname: '',
        author: '',
        publisher: '',
      },
      id: '',
      false: false,
      find: '',
      bookname: '',
      author: '',
      publisher: '',
    }
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val //拿到选中数据
      console.log(this.multipleSelection)
    },
    handleClick(row) {
      console.log(row)
    },
    findFn() {
      if (this.find.length == 0) return
      console.log(this.find)
      this.$axios({
        url: 'http://www.liulongbin.top:3006/api/getbooks',
        // method: 'GET',
        params: {
          bookname: this.find,
        },
      }).then((res) => {
        console.log(res.data.data)
        this.tableData = res.data.data
        // console.log(this.tableData);
      })
    },
    addFn() {
      if (
        this.objBook.bookname == '' ||
        this.objBook.author == '' ||
        this.objBook.publisher == ''
      )
        return alert('内容不能为空')
      this.flag = true
      this.$axios({
        url: 'http://www.liulongbin.top:3006/api/addbook',
        method: 'POST',
        data: {
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
          ...this.objBook,
        },
      }).then((res) => {
        console.log(res)
        const id = this.tableData[this.tableData.length - 1]
          ? this.tableData[this.tableData.length - 1].id + 1
          : 1
        this.tableData.push({
          id,
          bookname: this.objBook.bookname,
          author: this.objBook.author,
          publisher: this.objBook.publisher,
        })
        this.objBook.bookname = ''
        this.objBook.author = ''
        this.objBook.publisher = ''
        this.flag = false
      })
    },
    delFn(id) {
      console.log(id)
      this.$axios({
        url: 'http://www.liulongbin.top:3006/api/delbook',
        method: 'GET',
        params: {
          id: id,
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
        },
      }).then((res) => {
        this.$axios({
          url: 'http://www.liulongbin.top:3006/api/getbooks',
        }).then((res) => {
          // console.log(res.data.data)
          this.tableData = res.data.data
        })
        // this.findFn()
      })
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
