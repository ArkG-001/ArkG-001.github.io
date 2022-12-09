<!-- 组织架构 -->
// treeData: 树状渲染数据
// canDrag: 是否可拖拽
// defaultProps: 配置选项
// expandedkey: 默认展开的节点的 key 的数组
// @tree-node-click: 点击节点时触发
// @saveDrag: 保存拖拽后的结构

// 注意: 拖拽时会涉及业务代码
<template>
  <div class="app-container">
    <Tree
      :shrink-flag="shrinkFlag"
      :tree-data="treeData"
      :default-props="defaultProps"
      :expandedkey="expandedkey"
      :can-drag="canDrag"
      class="treeSty"
      :style="{width: leftWidth + 'px'}"
      @tree-node-click="treeNodeClick"
      @add-dept="addDept"
      @edit-dept="editDept"
      @del-dept="delDept"
      @saveDrag="saveDrag"
    />
  </div>
</template>

<script>
import Tree from '@/components/Tree'

export default {
  components: {
    Tree
  },
  data() {
    return {
      shrinkFlag: 1, // 0：宽1，1：宽280， 2：宽400
      treeData: [], // 树状数据
      defaultProps: {
        // 配置选项，具体看下表
        children: 'children',
        label: 'orgname'
      },
      expandedkey: [], // 默认展开的节点的 key 的数组
      canDrag: false, // 是否有拖拽功能

      list: [
        {
          DeptId: 1,
          DeptName: '总部',
          DeptDesc: '总部',
          DeptLeaderName: '',
          DeptLeaderId: '',
          ChildDept: [
            {
              DeptId: 2,
              DeptName: '研发部',
              DeptDesc: '研发部',
              DeptLeaderName: '谢次正',
              DeptLeaderId: '37',
              ChildDept: [
                {
                  DeptId: 8,
                  DeptName: '研发二部',
                  DeptDesc: '研发二部',
                  DeptLeaderName: '',
                  DeptLeaderId: '',
                  ChildDept: []
                },
                {
                  DeptId: 9,
                  DeptName: '研发三部',
                  DeptDesc: '研发三部',
                  DeptLeaderName: '哈',
                  DeptLeaderId: '1',
                  ChildDept: [
                    {
                      DeptId: 10,
                      DeptName: '研发三部1组',
                      DeptDesc: '研发三部一组',
                      DeptLeaderName: '小二',
                      DeptLeaderId: '16',
                      ChildDept: []
                    },
                    {
                      DeptId: 20,
                      DeptName: '研发一部',
                      DeptDesc: '98989',
                      DeptLeaderName: '简',
                      DeptLeaderId: '6',
                      ChildDept: []
                    },
                    {
                      DeptId: 24,
                      DeptName: '研发三组',
                      DeptDesc: '案件考虑到',
                      DeptLeaderName: '小一',
                      DeptLeaderId: '15',
                      ChildDept: []
                    }
                  ]
                },
                {
                  DeptId: 23,
                  DeptName: '研发一部1',
                  DeptDesc: '皇帝',
                  DeptLeaderName: '',
                  DeptLeaderId: '',
                  ChildDept: [
                    {
                      DeptId: 57,
                      DeptName: '制造部',
                      DeptDesc: '2323',
                      DeptLeaderName: '',
                      DeptLeaderId: '',
                      ChildDept: []
                    }
                  ]
                }
              ]
            },
            {
              DeptId: 3,
              DeptName: '制造部12',
              DeptDesc: '制造部',
              DeptLeaderName: '',
              DeptLeaderId: '',
              ChildDept: [
                {
                  DeptId: 19,
                  DeptName: '制造二部2',
                  DeptDesc: '00',
                  DeptLeaderName: '',
                  DeptLeaderId: '',
                  ChildDept: []
                }
              ]
            },
            {
              DeptId: 6,
              DeptName: '销售部2',
              DeptDesc: '销售部',
              DeptLeaderName: '',
              DeptLeaderId: '',
              ChildDept: [
                {
                  DeptId: 14,
                  DeptName: '销售一部',
                  DeptDesc: '销售一部11224坎坎坷坷',
                  DeptLeaderName: '',
                  DeptLeaderId: '',
                  ChildDept: []
                }
              ]
            },
            {
              DeptId: 60,
              DeptName: '总部三',
              DeptDesc: '',
              DeptLeaderName: '天台',
              DeptLeaderId: '20',
              ChildDept: []
            }
          ]
        }
      ],
      loding: false,

      leftWidth: 280 // 宽度
    }
  },
  watch: {
  },
  created() {
    this.getTreeData()
  },

  methods: {
    // 节点点击事件
    treeNodeClick(data, node, comment) {
      this.orguid = data.orguid
      console.log('查询根节点数据：')
      console.log(data.orguid)
      this.fetchData()
    },
    fetchData() {
      console.log('获取员工列表')
    },
    getTreeData() {
      // 获取数据
      console.log('获取数据')
      this.treeData = [ // 树状数据
        {
          orguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
          porguid: '00000000-0000-0000-0000-000000000000',
          orgtypeid: 1,
          datalevel: 1,
          rootorgname: '上海宏英智能科技股份有限公司',
          orgname: '上海宏英智能科技股份有限公司',
          orgfullpath: '',
          orgfullname: null,
          orgdesc: '上海宏英智能科技股份有限公司',
          orgicon: null,
          sortindex: 0,
          hasorgchild: 0,
          level: 0,
          btnright: 0,
          children: [
            {
              orguid: '329653cd-7236-cefe-018e-03a365001e19',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 10,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '测试部门',
              orgfullpath: '测试部门',
              orgfullname: 'test20220126',
              orgdesc: '1111111111111111111111',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: [
                {
                  orguid: 'a3318151-ddb3-e626-b60f-ec8545b5c65d',
                  porguid: '329653cd-7236-cefe-018e-03a365001e19',
                  orgtypeid: 10,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: 'test1',
                  orgfullpath: '测试部门-test1',
                  orgfullname: 'test1',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,

                  children: [
                    {
                      orguid: '2950aced-9105-f817-855f-30f81898c5b0',
                      porguid: 'a3318151-ddb3-e626-b60f-ec8545b5c65d',
                      orgtypeid: 10,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '444',
                      orgfullpath: '测试部门-test1-444',
                      orgfullname: '444',
                      orgdesc: null,
                      orgicon:
                        '/Content/logo/2022-02-17/5b32c20a-ece7-4a7f-9073-b340ba40a315.png',
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'c1e09df8-3a55-4a95-5a01-4e77ffe91fe3',
                      porguid: 'a3318151-ddb3-e626-b60f-ec8545b5c65d',
                      orgtypeid: 10,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '112',
                      orgfullpath: '测试部门-test1-112',
                      orgfullname: '112',
                      orgdesc: null,
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'b90bf824-2c48-751b-cdda-f30f678c798e',
                      porguid: 'a3318151-ddb3-e626-b60f-ec8545b5c65d',
                      orgtypeid: 10,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '3333',
                      orgfullpath: '测试部门-test1-3333',
                      orgfullname: '3333',
                      orgdesc: null,
                      orgicon:
                        '/Content/logo/2022-01-27/7a256c99-d576-4697-8ffd-4b3c99162346.png',
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    }
                  ]
                }
              ]
            },
            {
              orguid: '57a25099-558e-2521-fc2c-12548a2aac41',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '中联重工',
              orgfullpath: '中联重工',
              orgfullname: '中联重科履带吊起重机有限公司',
              orgdesc: '中联重工',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'd0950312-b8d0-b653-ef11-164f0900817c',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '徐工建机',
              orgfullpath: '徐工建机',
              orgfullname: null,
              orgdesc: '徐工建机',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'b16e5684-ff1f-4257-982e-26a388dffdcd',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '大庆石油',
              orgfullpath: '大庆石油',
              orgfullname: '大庆石油',
              orgdesc: null,
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'a49f1c99-b9cc-7d68-c7b4-3a1be4cbceb6',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '1.2.1',
              orgfullpath: '1.2.1',
              orgfullname: null,
              orgdesc: '1.2.1',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'c77b738f-2e58-c2a3-d2ce-5cc15bd70e59',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '三一塔机',
              orgfullpath: '三一塔机',
              orgfullname: null,
              orgdesc: '三一塔机',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: '9f5ea2a3-edf5-d976-c442-65659448ee41',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: 'JT',
              orgfullpath: 'JT',
              orgfullname: '测试-沈阳捷通',
              orgdesc: null,
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '沈阳捷通',
              orgfullpath: '沈阳捷通',
              orgfullname: '捷通云',
              orgdesc: '沈阳捷通',
              orgicon:
                '/Content/logo/2021-12-21/a4fa58ba-91c2-4300-b843-34917446cd94.jpg',
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: [
                {
                  orguid: '717e1d68-3fac-576b-818b-1b92e0d841a6',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 0,
                  datalevel: 0,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '支队',
                  orgfullpath: '沈阳捷通-支队',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                },
                {
                  orguid: '2824e283-a74b-422d-0e8a-275defdeb8fc',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '售后部',
                  orgfullpath: '沈阳捷通-售后部',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: [
                    {
                      orguid: 'fa5b6a69-480c-1d64-c016-0483ebc38602',
                      porguid: '2824e283-a74b-422d-0e8a-275defdeb8fc',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '售后主管',
                      orgfullpath: '沈阳捷通-售后部-售后主管',
                      orgfullname: '售后部',
                      orgdesc: '负责：售后信息的填报；',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'de3c1d58-3661-d17d-6d76-3eb5e1e30097',
                      porguid: '2824e283-a74b-422d-0e8a-275defdeb8fc',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '售后员',
                      orgfullpath: '沈阳捷通-售后部-售后员',
                      orgfullname: '售后部',
                      orgdesc: '负责：现场维修并填报修后单',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: '9bf073ba-864d-27af-b35d-c847c4e8dd68',
                      porguid: '2824e283-a74b-422d-0e8a-275defdeb8fc',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '售后客服',
                      orgfullpath: '沈阳捷通-售后部-售后客服',
                      orgfullname: '售后部',
                      orgdesc:
                        '负责：处理在线报修；给售后人员分派维修单；监督售后任务执行；',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    }
                  ]
                },
                {
                  orguid: '36847384-24b6-df51-e06c-28870dd38128',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '技术部',
                  orgfullpath: '沈阳捷通-技术部',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                },
                {
                  orguid: '0ebd16f6-4e2e-adab-0bc9-32dce59b3d44',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '调试部',
                  orgfullpath: '沈阳捷通-调试部',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: [
                    {
                      orguid: 'e62b6434-6949-0d33-1e1c-3b6e7045e3c8',
                      porguid: '0ebd16f6-4e2e-adab-0bc9-32dce59b3d44',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '调试主管',
                      orgfullpath: '沈阳捷通-调试部-调试主管',
                      orgfullname: '调试部',
                      orgdesc: '负责： EBOX设备绑定；调试流程管理',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'd6b84cf7-61f4-9db4-23cc-7638d44eae5c',
                      porguid: '0ebd16f6-4e2e-adab-0bc9-32dce59b3d44',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '调试员',
                      orgfullpath: '沈阳捷通-调试部-调试员',
                      orgfullname: '调试部',
                      orgdesc: '负责：调试信息的填报',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: '24ffa89d-92c9-59cc-8eb5-cb9f22716978',
                      porguid: '0ebd16f6-4e2e-adab-0bc9-32dce59b3d44',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '检查员',
                      orgfullpath: '沈阳捷通-调试部-检查员',
                      orgfullname: '调试部',
                      orgdesc: '负责：调试过程中完成进度的填报',
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    }
                  ]
                },
                {
                  orguid: '56c8fdfb-65be-ee51-990c-41b6ab1faf29',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 0,
                  datalevel: 0,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '中队',
                  orgfullpath: '沈阳捷通-中队',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                },
                {
                  orguid: '39121e7d-a64b-044d-9002-8c4f0ef6db55',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '销售部',
                  orgfullpath: '沈阳捷通-销售部',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: [
                    {
                      orguid: '8ed2b0e0-239d-850a-8fda-879eef150e2a',
                      porguid: '39121e7d-a64b-044d-9002-8c4f0ef6db55',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '销售经理',
                      orgfullpath: '沈阳捷通-销售部-销售经理',
                      orgfullname: '销售部',
                      orgdesc: null,
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'e5fa405d-fe34-566a-86cf-d744bc927cd7',
                      porguid: '39121e7d-a64b-044d-9002-8c4f0ef6db55',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '销售主管',
                      orgfullpath: '沈阳捷通-销售部-销售主管',
                      orgfullname: '销售部',
                      orgdesc: null,
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    },
                    {
                      orguid: 'eb0d8003-5913-5cc1-4bd0-fc646c0772d3',
                      porguid: '39121e7d-a64b-044d-9002-8c4f0ef6db55',
                      orgtypeid: 1,
                      datalevel: 1,
                      rootorgname: '上海宏英智能科技股份有限公司',
                      orgname: '销售内勤',
                      orgfullpath: '沈阳捷通-销售部-销售内勤',
                      orgfullname: '销售部',
                      orgdesc: null,
                      orgicon: null,
                      sortindex: 0,
                      hasorgchild: 0,
                      level: 3,
                      btnright: 1,
                      children: []
                    }
                  ]
                },
                {
                  orguid: 'ad9f4133-9a6e-2fc8-0bb9-99d66fc40141',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 0,
                  datalevel: 0,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '大队',
                  orgfullpath: '沈阳捷通-大队',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                },
                {
                  orguid: 'f43704e8-5475-88af-a4eb-f7fa131efbf9',
                  porguid: '1e2e2d7d-7474-cb8f-8b5a-6d905fe58dbe',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '信息部',
                  orgfullpath: '沈阳捷通-信息部',
                  orgfullname: '沈阳捷通',
                  orgdesc: null,
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                }
              ]
            },
            {
              orguid: '5cdf7b3a-8c4b-7e49-0136-796aa1472eda',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '塔机项目组',
              orgfullpath: '塔机项目组',
              orgfullname: null,
              orgdesc: '塔机项目组',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'b146078f-a1a7-e07e-338a-919006224606',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '2',
              orgfullpath: '2',
              orgfullname: null,
              orgdesc: '2',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: [
                {
                  orguid: '268bacee-9c2e-0614-0f95-1a5107509fec',
                  porguid: 'b146078f-a1a7-e07e-338a-919006224606',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '2.1',
                  orgfullpath: '2-2.1',
                  orgfullname: null,
                  orgdesc: '2.1',
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                }
              ]
            },
            {
              orguid: 'ac4ee7b5-ae1a-0263-eb95-cd3c587e1124',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '1.2.1.1',
              orgfullpath: '1.2.1.1',
              orgfullname: null,
              orgdesc: '1.2.1.1',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: 'dbebc750-6d51-aa62-c9ae-d5f7b5a31dd9',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '宏英云演示平台',
              orgfullpath: '宏英云演示平台',
              orgfullname: '宏英云演示平台',
              orgdesc: null,
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: '1893d0d8-8e1a-cca9-da13-e8f1e9a1a091',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '山河智能',
              orgfullpath: '山河智能',
              orgfullname: '山河智能',
              orgdesc: '山河智能',
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: []
            },
            {
              orguid: '0a1abf68-7474-e5a3-b1d4-f24f0f07a6bf',
              porguid: '08fbda05-9bab-4a1c-87ac-208bf5cbc917',
              orgtypeid: 1,
              datalevel: 1,
              rootorgname: '上海宏英智能科技股份有限公司',
              orgname: '久隆项目组',
              orgfullpath: '久隆项目组',
              orgfullname: '久隆项目组',
              orgdesc: null,
              orgicon: null,
              sortindex: 0,
              hasorgchild: 0,
              level: 1,
              btnright: 1,
              children: [
                {
                  orguid: '05eed511-48cc-507a-a148-59ba74579ff4',
                  porguid: '0a1abf68-7474-e5a3-b1d4-f24f0f07a6bf',
                  orgtypeid: 1,
                  datalevel: 1,
                  rootorgname: '上海宏英智能科技股份有限公司',
                  orgname: '久隆财产保险',
                  orgfullpath: '久隆项目组-久隆财产保险',
                  orgfullname: '久隆财产保险',
                  orgdesc: '久隆财产保险',
                  orgicon: null,
                  sortindex: 0,
                  hasorgchild: 0,
                  level: 2,
                  btnright: 1,
                  children: []
                }
              ]
            }
          ]
        }
      ]
    },
    addDept() {
      console.log('新增部门')
    },
    editDept() {
      console.log('编辑部门')
    },
    delDept() {
      console.log('删除部门')
    },
    saveDrag(selfOrgId, fuOrgId) {
      // 保存拖拽
      console.log('保存拖拽')
    }

  }
}
</script>

<style scoped lang="scss">
.treeSty {
    background-color: #fff;
    height: calc(100vh - 180px);
    border-right:1px solid #fff;
    // display: inline-block;
    float: left;
    // width: 40%;
    // height: 50%;
  }

</style>

