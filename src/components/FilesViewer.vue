<template>
  <table class="table">
    <tbody>
      <tr class="clickable" @click="$emit('back')">
        <td class="icon-row">
          <icon-folder-open class="icon-folder" />
        </td>
        <td>..</td>
        <td></td>
      </tr>
      <tr
        v-for="file in files"
        :key="file.name"
        :class="{ clickable: file.directory }"
        @click="onFileClick(file)"
      >
        <td class="icon-row">
          <icon-folder v-if="file.directory" class="icon-folder" />
          <icon-file v-else class="icon-file" />
        </td>
        <td>{{ file.name }}</td>
        <td>
          <span class="float-end">{{ file.size }}</span>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import IconFile from './IconFile'
import IconFolder from './IconFolder'
import IconFolderOpen from './IconFolderOpen'
defineProps({
  files: Array
})

const emit = defineEmits()
const onFileClick = (file) => {
  if (file.directory) {
    emit('folderclick', file)
  }
}
</script>

<style scoped>
.clickable {
  cursor: pointer;
}
.icon-row {
  width: 2em;
}
.icon-folder {
  width: 40px;
}
.icon-file {
  width: 0.75em;
}
</style>