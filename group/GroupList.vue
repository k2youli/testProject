<!-- 用户组列表 -->
<template>
  <div style="height: 100%;">
    <Table
      :columns="group.columns"
      :data="group.data"
      size="small"
      :loading="group.loading"
      class="margin-bottom"></Table>
    <Page
      :total="group.total"
      class="page-container"
      @on-change="onPageChange"
    />
  </div>
</template>
<script>
// eslint-disable-next-line
import { Table, Page, Icon } from 'iview'

import { api } from '../api'

export default {
  name: 'GroupList',
  components: {
    Table,
    Page
  },
  props: {
    currentRole: {
      type: Object,
      required: false,
      default: () => {
        return {}
      }
    },
    isReloadGroupList: {
      type: Boolean,
      required: false
    }
  },
  data () {
    return {
      group: {
        page: 1,
        size: 10,
        total: 0,
        loading: false,
        data: [],
        columns: [
          { title: '名称', key: 'name', minWidth: 110 },
          { title: '描述', key: 'email', minWidth: 130 },
          {
            title: '操作',
            width: 100,
            render: (h, params) => {
              return h(
                'span',
                {
                  style: {
                    cursor: 'pointer'
                  },
                  on: {
                    click: () => {
                      this.deleteUser(params.row.name)
                    }
                  }
                },
                [
                  h(
                    'Icon',
                    {
                      props: {
                        type: 'ios-trash',
                        size: 16,
                        color: '#5cadff'
                      }
                    }
                  ),
                  h(
                    'span', '移除'
                  )
                ]
              )
            }
          }
        ]
      }
    }
  },
  watch: {
    currentRole: {
      handler (curVal, oldVal) {
        if (curVal && curVal.id) {
          this.getGroupList()
        }
      }
    },
    isReloadGroupList: {
      handler (curVal, oldVal) {
        if (this.currentRole) {
          // 添加用户组后刷新用户组列表页面
          this.getGroupList()
        }
      }
    }
  },
  mounted () {
    if (this.currentRole) {
      this.getGroupList()
    }
  },
  methods: {
    // 删除用户组
    deleteGroup (name) {
      this.$axios.put(`${api.roles}/${this.currentRole.id}/remove`, { users: [{ name: name }] }).then(res => {
        this.getGroupList()
      })
    },
    // 获取用户组列表
    getGroupList () {
      this.group.loading = true
      let { page, size } = this.group
      let name = this.currentRole.name

      this.$axios.get(`${api.users}/?page=${page}&size=${size}&rolename=${name}`).then(res => {
        this.group.data = res.data.result
        this.group.total = res.data.pages.total
        this.group.loading = false
      })
    },
    // 切换页数
    onPageChange (page) {
      this.group.page = page
      this.getGroupList()
    }
  }
}
</script>
