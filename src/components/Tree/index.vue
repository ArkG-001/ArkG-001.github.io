<!-- 组织架构 -->
<template>
  <div>
    <!-- 搜索筛选 -->
    <el-input
      v-show="shrinkFlag != 0"
      v-model="filterText"
      size="medium"
      placeholder="输入关键字进行过滤"
    />
    <!-- 是否可拖拽 -->
    <div
      v-show="shrinkFlag != 0"
      style="padding-top: 10px; padding-bottom: 10px"
    >
      <el-switch
        v-if="canDrag"
        v-model="draggable"
        active-text="开启拖拽"
        inactive-text="关闭拖拽"
      />
    </div>
    <!-- 树状结构 -->
    <div class="treeArea tree-container">
      <el-tree
        ref="menuTree"
        class="tree"
        :data="treeData"
        :props="defaultProps"
        :expand-on-click-node="false"
        :default-expanded-keys="expandedkey"
        node-key="orguid"
        :draggable="draggable"
        :allow-drop="allowDrop"
        :filter-node-method="filterNode"
        :highlight-current="true"
        default-expand-all
        @node-drop="handleDrop"
        @node-click="nodeclick"
      >
        <span slot-scope="{ node, data }" class="custom-tree-node">
          <span>
            <i :class="data.orgicon" style="margin-right: 2px" />{{
              node.label
            }}</span>
          <!-- 右侧操作按钮 -->
          <span style="width: 70px">
            <el-tooltip
              class="item"
              effect="dark"
              content="添加"
              placement="top"
            >
              <i
                class="fontColor el-icon-circle-plus-outline"
                style="font-size: 16px; width: 16px"
                type="text"
                size="mini"
                @click.stop="() => append(node, data)"
              />
            </el-tooltip>
            <el-tooltip
              class="item"
              effect="dark"
              content="编辑"
              placement="top"
            >
              <i
                v-if="node.level !== 1"
                style="margin: 0 10px; font-size: 16px; width: 16px"
                class="fontColor el-icon-edit"
                type="text"
                size="mini"
                @click.stop="() => edit(data)"
              />
            </el-tooltip>
            <el-tooltip
              class="item"
              effect="dark"
              content="删除"
              placement="top"
            >
              <i
                v-if="node.childNodes.length == 0"
                style="color: #f34d37; font-size: 16px"
                class="el-icon-delete"
                type="text"
                size="mini"
                @click.stop="() => remove(node, data)"
              />
            </el-tooltip>
          </span>
        </span>
      </el-tree>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    shrinkFlag: {
      type: Number,
      default: () => 1
    },
    treeData: {
      type: Array,
      default: () => []
    },
    defaultProps: {
      type: Object,
      default: () => {}
    },
    expandedkey: {
      type: Array,
      default: () => []
    },
    canDrag: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      filterText: '', // 树形过滤
      draggable: false, // 是否可拖拽
      maxLevel: 1,
      // 业务数据
      selfOrgId: '',
      fuOrgId: ''
    }
  },
  watch: {
    filterText(val) {
      this.$refs.menuTree.filter(val)
    }
  },
  methods: {
    // 点击末级节点，给父组件传参，查询对应员工
    nodeclick(data, node, component) {
      this.$emit('tree-node-click', data, node, component)
    },
    // 树节点过滤
    filterNode(value, data) {
      if (!value) return true
      return data.orgname.indexOf(value) !== -1
    },
    // 添加部门
    append(node, data) {
      this.$emit('add-dept', node, data)
    },
    // 修改部门
    edit(data) {
      // 查询 组织类别
      this.$emit('edit-dept', data)
    },
    // 删除部门
    remove(node, data) {
      this.$emit('del-dept', node, data)
    },
    // 是否允许拖拽
    allowDrop(draggingNode, dropNode, type) {
      // 前提约束：菜单总层数不能大于三层
      // 判断：被拖动的当前节点层数+父节点层数 <= 3
      // 获取当前节点总层数
      this.countNodeLevel(draggingNode)
      const deep = Math.abs(this.maxLevel - draggingNode.level) + 1
      if (type === 'inner') {
        // return deep + dropNode.level <= 3;
        return deep + dropNode.level <= 40
      } else {
        // return deep + dropNode.parent.level <= 3;
        return deep + dropNode.parent.level <= 40
      }
    },
    // 获取当前节点层数
    countNodeLevel(node) {
      if (node.childNodes != null && node.childNodes.length > 0) {
        for (let i = 0; i < node.childNodes.length; i++) {
          if (node.childNodes[i].level > this.maxLevel) {
            this.maxLevel = node.childNodes[i].level
          }
          this.countNodeLevel(node.childNodes[i])
        }
      } else {
        this.maxLevel = node.level
      }
    },
    // 拖拽完成触发
    handleDrop(draggingNode, dropNode, dropType, ev) {
      console.log('拖拽完成触发')
      if (dropType === 'before' || dropType === 'after') {
        // 同级
        this.selfOrgId = draggingNode.data.orguid
        this.fuOrgId = dropNode.data.porguid
        if (dropNode.data.level === 0) {
          this.$message.warning('根节点不能改变！')
          // 获取最新数据
          this.$emit('getTreeData')
          return
        }
      } else if (dropType === 'inner') {
        this.selfOrgId = draggingNode.data.orguid
        this.fuOrgId = dropNode.data.orguid
        if (dropNode.data.level === 0) {
          this.$message.warning('根节点不能改变！')
          // 获取最新数据
          this.$emit('getTreeData')
          return
        }
      }
      // 发请求保存更改
      // const params = {
      //   CallType: 'drag',
      //   UserID: this.UserID,
      //   orguid: this.selfOrgId,
      //   porguid: this.fuOrgId
      // }
      this.$emit('saveDrag', this.selfOrgId, this.fuOrgId)
      this.maxLevel = 0
    }
  }
}
</script>
<style lang="scss" scoped>
.fontColor {
  color: #00a06e;
}

.custom-tree-node {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  padding-right: 15px;
}
::v-deep.treeArea {
  width: 100%;
  height: calc(100% - 90px);
  overflow-y: scroll;
  overflow-x: hidden !important;
}

/* 树形结构节点添加连线 */
.tree ::v-deep .el-tree-node {
  position: relative;
}
.tree ::v-deep .el-tree-node__children {
  padding-left: 26px;
}
.tree ::v-deep .el-tree-node :last-child:before {
  // height: 38px;
  height: 12px;
}
.tree ::v-deep .el-tree > .el-tree-node:before {
  border-left: none;
}
.tree-container ::v-deep .el-tree > .el-tree-node:after {
  border-top: none;
}
.tree ::v-deep .el-tree-node:before {
  content: "";
  left: -4px;
  position: absolute;
  right: auto;
  // border-width: 1px;
  border-left: 1px dashed #b8b9bb;
  width: 10px;
  height: 100%;
  bottom: 0px;
  // top: -26px;
  top: 0px;
}
.tree ::v-deep .el-tree-node:after {
  content: "";
  left: -4px;
  position: absolute;
  right: auto;
  // border-width: 1px;
  border-top: 1px dashed #b8b9bb;
  width: 12px;
  height: 20px;
  top: 12px;
}
.tree ::v-deep .el-tree-node__expand-icon.is-leaf {
  display: none;
}
.tree ::v-deep .el-tree-node__content {
  padding-left: 12px !important;
}
.tree ::v-deep .el-tree-node__content > .el-tree-node__expand-icon {
  padding: 0px !important;
}
//修改图标大小
.tree ::v-deep .el-icon-caret-right:before {
  content: "\e791";
  font-size: 16px;
}

/* 设置 鼠标悬浮节点时 的背景颜色 */
// .tree ::v-deep .el-tree-node__content:hover {
  // background-color: #2B2B2B;
// }
.tree ::v-deep .el-tree-node__content {
  padding-left: 12px !important;
}

/* 设置三角图表的 颜色 */
.tree ::v-deep .el-tree-node__content > .el-tree-node__expand-icon {
  padding: 0px !important;
  // color: #fff;
}

// 设置节点点击后的 样式
.tree ::v-deep .el-tree-node:focus > .el-tree-node__content {
  color: #00a06e !important;
  // background-color: #2B2B2B !important;
}

// 鼠标点击后,再点击空白地方,节点失去焦点时显示的背景色
::v-deep.el-tree--highlight-current
  .el-tree-node.is-current
  > .el-tree-node__content {
  // background-color: #2B2B2B !important;
  color: #00a06e !important;
}
</style>
