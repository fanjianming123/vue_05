<template>
  <div>
    <el-input
      v-model.trim="search"
      style="width: 500px"
      placeholder="输入书名搜索"
    />
    <el-button
      type="primary"
      icon="el-icon-search"
      style="margin-right: 200px"
      @click="searchBook"
      >搜索</el-button
    >
    <el-button type="text" @click="addBook" :disabled="flag"
      >添加图书</el-button
    >
    <el-dialog title="添加图书" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="书名" :label-width="formLabelWidth">
          <el-input v-model.trim="form.bookname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="作者" :label-width="formLabelWidth">
          <el-input v-model.trim="form.author" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="出版社" :label-width="formLabelWidth">
          <el-input v-model.trim="form.publisher" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel">取 消</el-button>
        <el-button type="primary" @click="add">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog title="图书信息" :visible.sync="dialogVisible" width="50%">
      <div v-if="searchData.length===0">获取图书失败，请稍后再试</div>
      <div v-if="searchData.length!==0">
        <ul>
          <li v-for="item in searchData" :key="item.id">
            序号：{{ item.id }}  书名：{{ item.bookname }}  作者：{{
              item.author
            }}  出版社：{{ item.publisher }}
          </li>
        </ul>
      </div>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false"
          >确 定</el-button
        >
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      search: '',
      dialogTableVisible: false,
      dialogFormVisible: false,
      dialogVisible: false,
      form: {
        bookname: '',
        author: '',
        publisher: '',
      },
      searchData: [],
      formLabelWidth: '120px',
      flag: false,
    }
  },
  mounted() {},
  computed: {},
  methods: {
    addBook() {
      this.dialogFormVisible = true
      this.flag = true
    },
    add() {
      if (!this.form.bookname || !this.form.author || !this.form.publisher)
        return alert('please enter')
      this.dialogFormVisible = false
      this.$axios({
        method: 'POST',
        url: '/api/addbook',
        data: this.form,
      })
        .then((res) => {
          this.flag = false
          console.log(res.data)
          if (res.data.status !== 201)
            return this.$message.error('添加图书失败，请稍后再试！！')
        })
        .then(
          this.$message({
            message: '添加图书成功',
            type: 'success',
          })
        ).then(() => location.reload())
      this.form.bookname = ''
      this.form.author = ''
      this.form.publisher = ''
    },
    cancel() {
      this.dialogFormVisible = false
      this.flag = false
    },
    searchBook() {
      if (!this.search) return
      this.dialogVisible = true
      this.$axios({
        url: '/api/getbooks',
        method: 'GET',
        params: {
          bookname: this.search,
        },
      }).then((res) => {
        console.log(res)
        if (res.data.status !== 200)
          return this.$message.error('查询图书失败，请稍后再试！！')
        this.searchData = res.data.data
        // console.log(this.searchData)
      })
      this.search=''
    },
  },
}
</script>
<style scoped></style>
