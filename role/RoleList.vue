<!-- 角色列表信息 -->
<template>
  <CellGroup @on-click="onSelectClick" class="left-list-pane">
    <Cell
      :selected="currentRole && roleItem.id === currentRole.id"
      :name="roleItem.id"
      v-for="roleItem in roleData"
      :key="roleItem.id">
      <Icon type="ios-people" slot="icon" size="18" color="#5cadff" />
      <span
        class="c-pointer"
        slot="label"
      >{{ roleItem.name }}</span>
      <span slot="extra" @click="onDeleteClick(roleItem.id)"><Icon type="ios-trash" size="16" color="#5cadff" /></span>
    </Cell>
  </CellGroup>
</template>
<script>
import { Icon, CellGroup, Cell } from 'iview'

export default {
  name: 'RoleList',
  components: {
    Icon,
    CellGroup,
    Cell
  },
  props: {
    roleData: {
      type: Array,
      required: false,
      default: () => {
        return []
      }
    },
    currentRole: {
      type: Object,
      required: false,
      default: () => {
        return {}
      }
    }
  },
  data () {
    return {
    }
  },
  methods: {
    onDeleteClick (id) {
      this.$emit('on-delete', id)
    },
    onSelectClick (id) {
      this.$emit('on-select', id)
    }
  }
}
</script>
