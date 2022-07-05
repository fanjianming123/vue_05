<template>
  <div>
    <ListHeader></ListHeader>
    <ListBody :tableData="tableData"></ListBody>
    <ListFooter></ListFooter>
  </div>
</template>

<script>
import ListHeader from './components/ListHeader.vue'
import ListBody from './components/ListBody.vue'
import ListFooter from './components/ListFooter.vue'
export default {
  data() {
    return {
      tableData: [
        {
          id: '1',
          bookname: '王小虎',
          author: '上海市普陀区金沙江路 1518 弄',
          publisher: '上海市普陀区金沙江路 1518 弄',
        },
        {
          id: '2',
          bookname: '王小虎',
          author: '上海市普陀区金沙江路 1517 弄',
          publisher: '上海市普陀区金沙江路 1518 弄',
        },
        {
          id: '3',
          bookname: '王小虎',
          author: '上海市普陀区金沙江路 1519 弄',
          publisher: '上海市普陀区金沙江路 1518 弄',
        },
        {
          id: '4',
          bookname: '王小虎',
          author: '上海市普陀区金沙江路 1516 弄',
          publisher: '上海市普陀区金沙江路 1518 弄',
        },
      ],
  
    }
  },
  components: {
    ListHeader,
    ListBody,
    ListFooter,
  },
  mounted() {},
  computed: {},
  methods: {
    handleEdit(index, row) {
      console.log(index, row)
    },
    handleDelete(index, row) {
      console.log(index, row)
    },
  },
}
</script>
<style scoped></style>
