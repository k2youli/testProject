<template>
  <Card class="container">
    <Split v-model="split">
      <div slot="left">
        <RoleInfo @on-role-change="getCurrentRole" />
      </div>
      <div slot="right">
        <Tabs
          type="line"
          v-model="currentTab">
          <div slot="extra">
            <Button
              type="primary"
              style="margin: 20px"
              v-if="currentTab === 'auth'"
              @click="isShowAuthModal = true"
            >
              添加权限
            </Button>
            <Button
              type="primary"
              style="margin: 20px"
              v-if="currentTab === 'group'"
              @click="isShowGroupModal = true"
            >
              添加已有用户组
            </Button>
          </div>
          <TabPane
            label="用户组"
            name="group"
            class="tab-pane">
            <GroupList :currentRole="currentRole" :isReloadGroupList="isReloadGroupList" />
            <GroupEdit
              :currentRole="currentRole"
              :isShowGroupModal="isShowGroupModal"
              v-if="currentRole"
              @on-submit="reloadUserList"
              @on-close="isShowGroupModal = false" />
          </TabPane>
          <TabPane
            label="权限"
            name="auth"
            class="tab-pane">
            <ResourceInfo
              :currentRole="currentRole"
              :resourceTypeList="resourceTypeList"
              :permission="permission" />
            <ResourceEdit
              :currentRole="currentRole"
              :resourceTypeList="resourceTypeList"
              :isShowAuthModal="isShowAuthModal"
              v-if="currentRole"
              @on-submit="getPermission"
              @on-close="isShowAuthModal = false" />
          </TabPane>
        </Tabs>
      </div>
    </Split>
  </Card>
</template>

<script>
import { Split, Button, Tabs, TabPane, Card } from 'iview'

import RoleInfo from './role'
import ResourceInfo from './resource'
import GroupList from './group'
import ResourceEdit from './resource/ResourceEdit.vue'
import GroupEdit from './group/GroupEdit.vue'

import { api } from './api'

export default {
  name: 'Authorization',
  components: {
    Split,
    Button,
    Tabs,
    TabPane,
    Card,
    RoleInfo,
    ResourceInfo,
    ResourceEdit,
    GroupList,
    GroupEdit
  },
  data () {
    return {
      split: 0.2,
      currentTab: 'group',
      isShowAuthModal: false,
      isShowGroupModal: false,
      isReloadGroupList: false,
      currentRole: null,
      resourceTypeList: [],
      permission: null
    }
  },
  mounted () {
    // 获取资源类型列表
    this.$axios.get(`${api.resourceTypes}?type=all`).then(res => {
      this.resourceTypeList = res.data.result
    })
  },
  methods: {
    getCurrentRole (currentRole) {
      this.currentRole = currentRole
    },
    getPermission (permission) {
      this.isShowAuthModal = false
      this.permission = permission
    },
    reloadUserList () {
      this.isShowGroupModal = false
      this.isReloadGroupList = !this.isReloadGroupList
    }
  }
}
</script>

<style lang="css" scoped>
.ivu-tabs {
  height: 100%;
}
</style>
