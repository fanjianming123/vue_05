<template>
  <div>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[5, 10, 15, 20]"
      :page-size="10"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>
<script>
import EventBus from "../EventBus/EventBus"
export default {
  data() {
    return {
      pageSize: 2,
      currentPage: 1,
      currentPage4: 2,
      total: 0,
    }
  },
  created() {
    this.$axios({
      url: '/api/getbooks',
    }).then((res) => {
      // console.log(res)
      return (this.total = res.data.data.length)
    })
    // EventBus.$emit('send', this.index, 1)
  },
  computed: {},
  methods: {
    handleSizeChange(val) {
      // this.$data.pageSize = val
      console.log(`每页 ${val} 条`)
      EventBus.$emit('handleSize', val)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
      EventBus.$emit('handleCurrent', val)
      // this.$data.currentPage = val
    },
  },
}
</script>
<style scoped></style>
