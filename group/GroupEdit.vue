<!-- 添加用户组对话框 -->
<template>
  <Modal
    title="添加已有用户组"
    width="700"
    v-model="isShowModal"
    @on-ok="onClickOk"
    @on-cancel="onClickCancel"
  >
    <K2Transfer
      :data="group.data"
      filterable
      :style="{width: '702px', margin: '0 auto'}"
      :list-style="{height: '400px', width: '300px'}"
      :target-keys="group.selectKeys"
      :selected-keys="group.selectKeys"
      :titles="group.titles"
      @on-change="handleChange"
      @on-dblclick="handleChange">
    </K2Transfer>
  </Modal>
</template>

<script>
import { Modal } from 'iview'
import K2Transfer from '@/components/kfc-k2transfer'

import { api } from '../api'

export default {
  name: 'GroupEdit',
  components: {
    Modal,
    K2Transfer
  },
  props: {
    currentRole: {
      type: Object,
      required: true
    },
    isShowGroupModal: {
      type: Boolean,
      required: true
    }
  },
  data () {
    return {
      isShowModal: this.isShowGroupModal,
      group: {
        titles: ['未选用户组', '已选用户组'],
        filterKey: 'id',
        filterLabel: 'name',
        data: [],
        selectKeys: []
      }
    }
  },
  watch: {
    isShowGroupModal: {
      handler (curVal, oldVal) {
        this.isShowModal = curVal
        // 清空选中参数
        this.group.selectKeys.splice(0, this.group.data.length)
        if (curVal) {
          this.getGroupList()
        }
      }
    }
  },
  methods: {
    // 添加用户组
    onClickOk () {
      this.$emit('on-close')
      // this.$axios.put(`${api.roles}/${this.currentRole.id}/add`, { users: users }).then(res => {
      //   this.$emit('on-submit')
      // })
    },
    onClickCancel () {
      this.$emit('on-close')
    },
    getGroupList () {
      this.$axios.get(`${api.groups}?type=all`).then(res => {
        this.group.data = res.data.result.map(item => {
          return {
            key: item.id,
            label: item.name
          }
        })
      })
    },
    handleChange (selection) {
      this.group.selectKeys = selection
    }
  }
}
</script>
