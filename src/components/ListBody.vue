<template>
  <div>
    <el-table
      :data="
        tableData.slice((currentPage - 1) * pageSize, currentPage * pageSize)
      "
      style="width: 100%"
    >
      <el-table-column label="序号" prop="id"> </el-table-column>
      <el-table-column label="书名" prop="bookname"> </el-table-column>
      <el-table-column label="作者" prop="author"> </el-table-column>
      <el-table-column label="出版社" prop="publisher"> </el-table-column>
      <el-table-column align="right">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">详情</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="书籍详情" :visible.sync="dialogVisible" width="50%">
      <p>序号：{{ editData.id }}</p>
      <p>书名：{{ editData.bookname }}</p>
      <p>作者：{{ editData.author }}</p>
      <p>出版社：{{ editData.publisher }}</p>
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
import EventBus from '../EventBus/EventBus'
export default {
  data() {
    return {
      dialogVisible: false,
      tableData: [],
      editData: {},
      pageSize: 10,
      currentPage: 1,
    }
  },
  created() {
    this.$axios({
      url: '/api/getbooks',
    }).then((res) => {
      return (this.tableData = res.data.data)
    })
    EventBus.$on('handleSize', (val) => (this.pageSize = val))
    EventBus.$on('handleCurrent', (val) => (this.currentPage = val))
  },
  computed: {},
  methods: {
    handleDelete(item) {
      console.log(item.id)
      this.$axios({
        url: `/api/delbook`,
        params: {
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
          id: item.id,
        },
      })
        .then(() => location.reload())
        .catch((err) => {
          this.$message.error(err.msg)
        })
    },
    handleEdit(item) {
      this.dialogVisible = true
      console.log(item)
      this.$axios({
        url: '/api/getbooks',
        params: {
          appkey: '7250d3eb-18e1-41bc-8bb2-11483665535a',
          id: item.id,
        },
      })
        .then((res) => {
          console.log(res)
          return (this.editData = res.data.data[0])
        })
        .catch((err) => {
          this.$message.error(err.msg)
        })
    },
  },
}
</script>
<style scoped></style>
