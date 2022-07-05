<template>
  <div>
    <template>
      <input
        type="text"
        placeholder="搜索-书本名称"
        style="width: 200px; height: 20px"
        @keydown.enter="enterFn"
        v-model.trim="books"
      />
      <el-table :data="bookList" border style="width: 100%">
        <el-table-column prop="id" label="序号" width="180"> </el-table-column>
        <el-table-column prop="bookname" label="书名" width="180">
        </el-table-column>
        <el-table-column prop="author" label="作者"> </el-table-column>
        <el-table-column prop="publisher" label="出版商"> </el-table-column>
        <el-table-column label="操作">
          <template v-slot="{ row }">
            <div>
              <button @click="clearFn(row.id)">删除</button>
              <button @click="detailsFn(row.id)">详情</button>
            </div>
          </template>
        </el-table-column>
      </el-table>
    </template>
    <template>
      <el-button type="text" @click="dialogFormVisible = true"
        >添加图书</el-button
      >
      <el-dialog title="书本信息" :visible.sync="dialogFormVisible">
        <el-form :model="form">
          <el-form-item label="书名" :label-width="formLabelWidth">
            <el-input
              v-model.trim="form.bookname"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <el-form-item label="作者" :label-width="formLabelWidth">
            <el-input v-model.trim="form.author" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="出版商" :label-width="formLabelWidth">
            <el-input
              v-model.trim="form.publisher"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <button
            style="margin-left: 300px; background: skyblue; width: 100px"
            @click.prevent="addFn(form)"
            :disabled="isShow"
          >
            确认
          </button>
        </el-form>
      </el-dialog>
    </template>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      isShow: false,
      books: '',
      bookList: [],
      dialogFormVisible: false,
      form: {
        bookname: '',
        author: '',
        publisher: '',
        type: [],
      },
      formLabelWidth: '120px',
    }
  },
  created() {
    axios({
      url: 'http://www.liulongbin.top:3006/api/getbooks',
    }).then((res) => {
      // console.log(res)
      this.bookList = res.data.data
      // console.log(this.bookList);
    })
  },
  methods: {
    enterFn() {
      axios({
        url: 'http://www.liulongbin.top:3006/api/getbooks',
        params: {
          bookname: this.books,
        },
      }).then((res) => {
        console.log(res)
        if (this.books == '') {
          return alert('输入框不能为空')
        }
        this.bookList = res.data.data
      })
    },
    addFn(val) {
      // console.log(val);
      if (
        this.form.bookname == '' ||
        this.form.author == '' ||
        this.form.publisher == ''
      ) {
        return alert('输入框不能为空')
      }
      this.isShow = true //添加后不能用
      axios({
        url: 'http://www.liulongbin.top:3006/api/addbook',
        method: 'POST',
        data: val,
      }).then(() => {
        // console.log(res)
        alert('添加图书成功')
        this.isShow = false //成功后恢复
      })
      ;(this.form.bookname = ''),
        (this.form.author = ''),
        (this.form.publisher = '')
    },
    clearFn(val) {
      console.log(val)
      axios({
        url: 'http://www.liulongbin.top:3006/api/delbook',
        method: 'GET',
        params: {
          id: val,
        },
      }).then((res) => console.log(res), alert('删除图书成功'))
    },
    detailsFn(id) {
      //根据id 调用api 接口 查询用户信息
      axios({
        url: 'http://www.liulongbin.top:3006/api/getbooks',
        params: {
          id,
        },
      }).then((res) => {
        // console.log(res)
        // console.log(JSON.stringify(res.data.data));
        alert(JSON.stringify(res.data.data))
      })
    },
  },
  watch: {
    bookList: {
      deep: true,
      immediate: true,
      handler() {
        axios({
          url: 'http://www.liulongbin.top:3006/api/getbooks',
        }).then((res) => {
          // console.log(res)
          this.bookList = res.data.data
          // console.log(this.bookList);
        })
      },
    },
  },
}
</script>
<style></style>
