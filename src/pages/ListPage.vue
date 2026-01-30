<template>
  <Page title="Task Tracker">
    <SelectAll
      v-if="items.length > 0"
      :selected-count="doneCount"
      :total-count="items.length"
      @toggle="toggleAllDone"
    >
      Mark All Done
    </SelectAll>

    <div class="list-container">
      <TransitionGroup name="list" tag="div">
        <Card
          v-for="item in items"
          :key="item.id"
          :status="item.status"
          @delete="removeItem(item.id)"
          @toggle-status="toggleStatus(item.id)"
        >
          <input
            v-if="item.editing"
            type="text"
            class="item-input"
            v-model="item.text"
            @blur="finishEditing(item)"
            @keyup.enter="finishEditing(item)"
            ref="editInput"
            autofocus
          />
          <span v-else class="item-text" @dblclick="startEditing(item)">
            {{ item.text }}
          </span>
        </Card>
      </TransitionGroup>

      <div v-if="items.length === 0" class="empty-state">
        <p>No tasks yet. Add one below!</p>
      </div>
    </div>

    <button class="add-btn" @click="addItem" title="Add new item">
      <span class="plus-icon">+</span>
    </button>
  </Page>
</template>

<script>
import Page from '../components/Page.vue'
import Card from '../components/Card.vue'
import SelectAll from '../components/SelectAll.vue'

export default {
  name: 'ListPage',
  components: {
    Page,
    Card,
    SelectAll
  },
  data() {
    return {
      items: [
        { id: 1, text: 'Learn Vue.js basics', status: 'done', editing: false },
        { id: 2, text: 'Build a task tracker', status: 'in-progress', editing: false },
        { id: 3, text: 'Add more features', status: 'pending', editing: false }
      ],
      nextId: 4
    }
  },
  computed: {
    doneCount() {
      return this.items.filter(item => item.status === 'done').length
    }
  },
  methods: {
    toggleStatus(id) {
      const item = this.items.find(item => item.id === id)
      if (item) {
        const statusOrder = ['pending', 'in-progress', 'done']
        const currentIndex = statusOrder.indexOf(item.status)
        const nextIndex = (currentIndex + 1) % statusOrder.length
        item.status = statusOrder[nextIndex]
      }
    },
    removeItem(id) {
      const index = this.items.findIndex(item => item.id === id)
      if (index !== -1) {
        this.items.splice(index, 1)
      }
    },
    addItem() {
      const newItem = {
        id: this.nextId++,
        text: 'New task',
        status: 'pending',
        editing: true
      }
      this.items.push(newItem)
      this.$nextTick(() => {
        const inputs = document.querySelectorAll('.item-input')
        if (inputs.length > 0) {
          inputs[inputs.length - 1].focus()
          inputs[inputs.length - 1].select()
        }
      })
    },
    startEditing(item) {
      item.editing = true
      this.$nextTick(() => {
        const inputs = document.querySelectorAll('.item-input')
        if (inputs.length > 0) {
          inputs[inputs.length - 1].focus()
        }
      })
    },
    finishEditing(item) {
      item.editing = false
      if (!item.text.trim()) {
        item.text = 'Untitled task'
      }
    },
    toggleAllDone(checked) {
      this.items.forEach(item => {
        item.status = checked ? 'done' : 'pending'
      })
    }
  }
}
</script>

<style scoped>
.list-container {
  min-height: 200px;
}

.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: #9ca3af;
}

.empty-state p {
  margin: 0;
  font-size: 16px;
}

.item-text {
  cursor: default;
}

.item-input {
  width: 100%;
  border: none;
  border-bottom: 2px solid #3b82f6;
  background: transparent;
  font-size: 14px;
  padding: 4px 0;
  outline: none;
  color: #374151;
}

.add-btn {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: #3b82f6;
  border: none;
  color: #fff;
  font-size: 32px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.add-btn:hover {
  background: #2563eb;
  transform: scale(1.1);
}

.add-btn:active {
  transform: scale(0.95);
}

.plus-icon {
  line-height: 1;
  margin-top: -2px;
}

/* List transition animations */
.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-move {
  transition: transform 0.3s ease;
}
</style>
