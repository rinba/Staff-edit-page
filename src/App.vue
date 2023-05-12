<script setup>
import axios from 'axios';
import Edit from './components/Edit.vue'
import {ref,onMounted} from 'vue'

//列表渲染
//步骤：声明响应式列表 -> 调用接口获取数据 -> 后端数据赋值给列表 ->绑定到table组件
const list = ref([])
const getList = async () =>{
  //接口调用
  const res = await axios.get('/list')
  list.value = res.data
}

onMounted(() => getList())

//删除功能
//步骤：获取当前行的id -> 通过id调用删除接口 -> 更新最新的列表
const onDelete = async (id) =>{
  //console.log(id) 验证
  await axios.delete(`/del/${id}`)
  getList()
}

//编辑功能

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <!--使用作用域插槽获取id，这里使用解构赋值row-->
        <template #default="{row}">
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
