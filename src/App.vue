<template>
  <div class="container mt-2">
    <h4>{{ path }}</h4>
    <div class="form-group mt-4 mb-2">
      <input
        class="form-control form-control-sm"
        placeholder="file search"
        v-model="searchString"
      />
      <files-viewer
        :files="filteredFiles"
        @back="back"
        @folderclick="open($event.name)"
      ></files-viewer>
    </div>
  </div>
</template>

<script setup>
import FilesViewer from './components/FilesViewer';
import fs from 'fs';
import { ref, computed } from 'vue';
import { app } from '@electron/remote';
import pathModule from 'path';

const path = ref(app.getAppPath())
const files = computed(() => {
  const filenames = fs.readdirSync(path.value)
  return filenames.map(file => {
    const stats = fs.statSync(pathModule.join(path.value, file))
    return {
      name: file, size: stats.isFile() ? formatSize(stats.size ?? 0) : null,
      directory: stats.isDirectory()
    }
  }).sort((a, b) => {
    if (a.directory === b.directory) {
      return a.name.localeCompare(b.name)
    }
    return a.directory ? -1 : 1;
  })
})

const back = () => {
  path.value = pathModule.dirname(path.value)
}
const open = (folder) => {
  path.value = pathModule.join(path.value, folder)
}

const searchString = ref('')
const filteredFiles = computed(() => {
  return searchString.value
    ? files.value.filter(s => s.name.includes(searchString.value))
    : files.value
})

const formatSize = (size) => {
  var i = Math.floor(Math.log(size) / Math.log(1024))
  return (
    (size / Math.pow(1024, i)).toFixed(2) * 1 + ' ' + ['B', 'KB', 'MB', 'GB', 'TB'][i]
  )
}
</script>

<style>
</style>
