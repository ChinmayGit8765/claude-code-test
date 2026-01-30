<template>
  <div class="page">
    <header class="page-header" v-if="title || $slots.header">
      <slot name="header">
        <h1 class="page-title">{{ title }}</h1>
      </slot>
    </header>
    <main class="page-content">
      <slot></slot>
    </main>
    <footer class="page-footer" v-if="$slots.footer">
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Page',
  props: {
    title: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f3f4f6;
}

.page-header {
  background: #fff;
  padding: 20px 24px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.page-title {
  margin: 0;
  font-size: 24px;
  font-weight: 600;
  color: #1f2937;
}

.page-content {
  flex: 1;
  padding: 24px;
  max-width: 800px;
  width: 100%;
  margin: 0 auto;
  box-sizing: border-box;
}

.page-footer {
  background: #fff;
  padding: 16px 24px;
  border-top: 1px solid #e5e7eb;
}
</style>
