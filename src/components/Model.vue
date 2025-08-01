<script setup>
import {onMounted, ref} from "vue";
import axios from "axios";


const tableData = ref([])

const listModel = () => {
  axios.get('http://localhost:8000/model/list').then(res => {
    tableData.value = res.data.data
  })
}

const createModel = () => {
  axios.get('http://localhost:8000/model/create').then(res => {
    if (res.data.code === 0) {
      const id = res.data.data
      window.location.href = `/modeler.html?modelId=${id}`
    }
  })
}

const editModel = (id) => {
  window.location.href = `/modeler.html?modelId=${id}`
}

const deleteModel = (id) => {
  axios.get(`http://localhost:8000/model/delete?id=${id}`).then(res => {
    listModel()
  })
}

onMounted(() => {
  listModel()
})

</script>

<template>
  <el-button type="success" @click="createModel">新建模型</el-button>
  <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="name" label="名字"/>
    <el-table-column prop="key" label="KEY"/>
    <el-table-column prop="createTime" label="创建时间"/>
    <el-table-column prop="lastUpdateTime" label="修改时间"/>
    <el-table-column prop="version" label="版本"/>
    <el-table-column prop="revision" label="修改"/>
    <el-table-column label="操作" min-width="120">
      <template #default="scope">
        <el-button link type="primary" size="small" @click="editModel(scope.row.id)">编辑</el-button>
        <el-button link type="primary" size="small" @click="deleteModel(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>

</template>

<style scoped>

</style>