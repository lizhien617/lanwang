<template>
  <div class="app-container">
    <el-table :data="roleList" style="width: 100%">
      <el-table-column prop="id" label="ID" width="180" />
      <el-table-column prop="name" label="角色名称" width="180" />
      <el-table-column prop="description" label="描述" />
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'UserRole',
  data() {
    return {
      roleList: [
        { id: 1, name: 'admin', description: '管理员' },
        { id: 2, name: 'editor', description: '编辑者' }
      ]
    }
  },
  methods: {
    handleEdit(row) {
      console.log('编辑', row)
    },
    handleDelete(row) {
      console.log('删除', row)
    }
  }
}
</script> 