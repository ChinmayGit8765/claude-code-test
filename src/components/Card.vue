<template>
  <div class="card" :class="statusClass">
    <button class="delete-btn" @click="$emit('delete')" title="Remove item">
      &times;
    </button>
    <div class="card-content">
      <button class="status-circle" @click="$emit('toggle-status')" :title="statusTitle">
        <span v-if="status === 'done'" class="checkmark">&#10003;</span>
        <span v-else-if="status === 'in-progress'" class="progress-dot"></span>
      </button>
      <div class="card-body">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    status: {
      type: String,
      default: 'pending',
      validator: (value) => ['pending', 'in-progress', 'done'].includes(value)
    }
  },
  emits: ['delete', 'toggle-status'],
  computed: {
    statusClass() {
      return `status-${this.status}`
    },
    statusTitle() {
      const titles = {
        'pending': 'Click to mark as in progress',
        'in-progress': 'Click to mark as done',
        'done': 'Click to reset to pending'
      }
      return titles[this.status]
    }
  }
}
</script>

<style scoped>
.card {
  position: relative;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 16px;
  padding-left: 20px;
  margin-bottom: 12px;
  transition: all 0.2s ease;
  border-left: 4px solid #e0e0e0;
}

.card.status-pending {
  border-left-color: #e0e0e0;
}

.card.status-in-progress {
  border-left-color: #f59e0b;
  background: #fffbeb;
}

.card.status-done {
  border-left-color: #10b981;
  background: #ecfdf5;
}

.card.status-done .card-body {
  text-decoration: line-through;
  opacity: 0.7;
}

.delete-btn {
  position: absolute;
  top: 4px;
  left: 4px;
  width: 20px;
  height: 20px;
  border: none;
  background: transparent;
  color: #9ca3af;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.delete-btn:hover {
  background: #fee2e2;
  color: #ef4444;
}

.card-content {
  display: flex;
  align-items: center;
  gap: 12px;
}

.status-circle {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: 2px solid #d1d5db;
  background: #fff;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.status-circle:hover {
  border-color: #9ca3af;
  transform: scale(1.1);
}

.status-pending .status-circle {
  border-color: #d1d5db;
}

.status-in-progress .status-circle {
  border-color: #f59e0b;
  background: #fef3c7;
}

.status-done .status-circle {
  border-color: #10b981;
  background: #10b981;
}

.checkmark {
  color: #fff;
  font-size: 14px;
  font-weight: bold;
}

.progress-dot {
  width: 10px;
  height: 10px;
  background: #f59e0b;
  border-radius: 50%;
}

.card-body {
  flex: 1;
  font-size: 14px;
  color: #374151;
}
</style>
