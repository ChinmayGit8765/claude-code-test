<template>
  <div class="select-all">
    <label class="select-all-label">
      <input
        type="checkbox"
        class="select-all-checkbox"
        :checked="isAllSelected"
        :indeterminate="isIndeterminate"
        @change="$emit('toggle', $event.target.checked)"
      />
      <span class="select-all-text">
        <slot>Select All</slot>
      </span>
    </label>
    <span class="select-all-count" v-if="showCount">
      {{ selectedCount }} / {{ totalCount }}
    </span>
  </div>
</template>

<script>
export default {
  name: 'SelectAll',
  props: {
    selectedCount: {
      type: Number,
      default: 0
    },
    totalCount: {
      type: Number,
      default: 0
    },
    showCount: {
      type: Boolean,
      default: true
    }
  },
  emits: ['toggle'],
  computed: {
    isAllSelected() {
      return this.totalCount > 0 && this.selectedCount === this.totalCount
    },
    isIndeterminate() {
      return this.selectedCount > 0 && this.selectedCount < this.totalCount
    }
  }
}
</script>

<style scoped>
.select-all {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 16px;
}

.select-all-label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  user-select: none;
}

.select-all-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #3b82f6;
}

.select-all-text {
  font-size: 14px;
  font-weight: 500;
  color: #374151;
}

.select-all-count {
  font-size: 13px;
  color: #6b7280;
  background: #f3f4f6;
  padding: 4px 10px;
  border-radius: 12px;
}
</style>
