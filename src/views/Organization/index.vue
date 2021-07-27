<template>
  <div class="white-body-view">
    <el-button type="primary" @click="addNewRecord()">新增</el-button>
    <custom-tree ref="customTree" :tree-data="treeData" :tree-expand-all="treeExpandAll" :tree-node-key="treeNodeKey" @addItem="addTreeItem" @deleteItem="deleteTreeItem" @editItem="editTreeItem" />
    <!-- 地点弹窗 -->
    <place-dialog ref="placeDialog" @addData="addData" @editData="editData" />
  </div>
</template>
<script>
import CustomTree from '@/components/Tree/Tree'
import PlaceDialog from '@/components/Tree/PlaceDialog'
export default {
  components: {
    CustomTree,
    PlaceDialog
  },
 
  data() {
    return {
      treeData: [],
      treeExpandAll: true,
      treeNodeKey: 'id'
    }
  },
 
  created() {
    this.initTreeData()
  },
 
  methods: {
    // 初始化列表
    initTreeData() {
      this.treeData = [
        {
          children: [
            {
              children: [
                {
                  children: [],
                  name: '监管管理局',
                  desc: '',
                  parentId: '2',
                  id: '9'
                }
              ],
              name: '合肥市',
              desc: '',
              parentId: '1',
              id: '2'
            },
            {
              children: [],
              name: '六安市',
              desc: '',
              parentId: '1',
              id: '3'
            },
            {
              children: [],
              name: '滁州市',
              desc: '',
              parentId: '1',
              id: '4'
            }
          ],
          name: '安徽省',
          parentId: '',
          id: '1'
        },
        {
          children: [
            {
              children: [],
              name: '济南市',
              desc: '',
              parentId: '5',
              id: '6'
            },
            {
              children: [
                {
                  children: [],
                  name: '济南市',
                  desc: '天桥区',
                  parentId: '7',
                  id: '8'
                }
              ],
              name: '青岛市',
              desc: '',
              parentId: '5',
              id: '7'
            }
          ],
          name: '山东省',
          parentId: '',
          id: '5'
        }
      ]
    },
 
    // 添加新记录
    addNewRecord() {
      this.$refs.placeDialog.openDialog(false)
    },
 
    // 新增表单数据
    addData(data) {
      // 新增树节点
      this.$refs.customTree.treeAddItem(data)
    },
 
    // 修改表单数据
    editData(data) {
      // 修改树节点
      this.$refs.customTree.treeEditItem(data)
    },
 
    // 增加树节点
    addTreeItem(data) {
      // 打开地点弹窗，设置默认父级节点
      this.$refs.placeDialog.openDialog(false, data.id)
    },
 
    // 删除树节点
    deleteTreeItem(data) {
      this.$confirm('确定删除吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 删除树节点
        this.$refs.customTree.treeDeleteItem(data)
        // 提示
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
 
    // 修改树节点
    editTreeItem(data) {
      // 打开地点编辑弹窗
      this.$refs.placeDialog.openDialog(true, data.parentId, data)
    }
 
  }
}
</script>
<style lang="scss">
  .white-body-view{
    width:30%;
  }
</style>
