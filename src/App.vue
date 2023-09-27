<script setup lang="ts">
import editorWorker from 'monaco-editor/esm/vs/editor/editor.worker?worker'
import tsWorker from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker'
import vueWorker from './vue.worker?worker'
import * as monaco from 'monaco-editor'
import { onMounted } from 'vue';
(window as any).MonacoEnvironment = {
  getWorker(_: any, label: string) {
    if (label === 'vue') {
      console.log('vue')
      return new vueWorker();
    }
    if (label === 'typescript') {
      console.log('typescript')
      return new tsWorker();
    }
    return new editorWorker();
  }
}

onMounted(() => {
  monaco.languages.register({ id: 'vue' })
  const editor = monaco.editor.create(document.querySelector('.editor') as any)

  const model = monaco.editor.createModel(`
<script>
  import {ref} from 'vue'
<\/script>

<template>
  div
<\/template>
  `, 'typescript', monaco.Uri.file('index.vue'))
  editor.setModel(model)
})
</script>

<template>
  <div class="editor"></div>
</template>

<style scoped>
.editor {
  height: 100%;
  width: 100%;
}
</style>
