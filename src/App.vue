<script setup>
import { onMounted, ref } from 'vue'
import Edit from './components/Edit.vue'
import $axios from 'axios'
// TODO: 列表渲染
// 思路 ：声明响应式 list -> 调用接口获取数据 -> 后端数据赋值给list -> 绑定table组件
const list  = ref([]) //定义响应式空列表

const getList = async ()=>{
  // ajax接口调用
  const res = await $axios.get('/list')
  // 赋值给list
  list.value = res.data
  console.log(res);
}
onMounted(()=>getList())

// TODO: 删除功能
// 思路： 获取当前列表的id -> 通过id 调用删除接口 -> 更新最新的列表

const onDelete = async (id)=>{
  console.log(id);
  await $axios.delete(`/del/${id}`)
  getList()
}

// TODO: 编辑功能
// 思路:  打开弹窗 -> 通过id请求编辑数据 -> 更新编辑后的列表 
// 1、打开弹窗（获取子组件实例，调用方法或者修改属性）
// 2、回填数据（调用详情接口，/当前行的静态数据）
const editRef = ref(null)
const onEdit = (row)=>{
  editRef.value.open(row)
  // console.log(editRef.value);
}
// 3、更新 移至Edit.vue中实现


</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{row}">
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <!-- //只更新列表  -->
  <Edit ref="editRef" @on-Update="getList"  />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
