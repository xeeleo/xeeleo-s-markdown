<script setup lang="ts">
import MarkDownEditor from './components/MarkDownEditor.vue'
import PageHeader from './components/PageHeader.vue'
</script>

<template>
  <div class="box">
    <PageHeader />
    <MarkDownEditor />
  </div>
</template>

<style scoped lang="scss">
.box {
  padding: 10px 20px;
}
@media screen and (min-width: 600px) {
  .box {
    padding: 20px 40px;
  }
}
</style>
