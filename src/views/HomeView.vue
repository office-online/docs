<script setup lang="ts">
let current_file = ref<File>('');

// 文件上传
function handleUpload(event) {
  current_file.value = event.target.files[0];
  if (!current_file.value) {
    return;
  }
  // 下拉框重置

  console.log(current_file.value);
}

// 重新加载
function handleReload(event) {
  if (!current_file.value) {
    return;
  }

  console.log(current_file.value);
}

// document容器
let document_container = ref<HTMLElement>(null);

// 保存测试
async function handleSave(event) {
  const file = await showSaveFilePicker({types: [{description: "HTML File", accept: {"text/html": [".html"]}}]});
  const stream = await file.createWritable();

  await stream.write(document_container.value.innerHTML);
  await stream.close();
}
</script>

<template>
  <main>
    <div class="bg-black p-3">
      <div class="text-center text-white">docx.renderSync function</div>
    </div>
    <div class="container-fluid">
      <div class="row gx-2 p-2 bg-light">
        <div class="col-auto">
          <input @change="handleUpload" type="file" class="form-control" accept=".docx"/>
        </div>
        <div class="col-auto">
          <select class="form-select">
            <option disabled selected>-- Test Documents --</option>
          </select>
        </div>
        <div class="col-auto">
          <button @click="handleReload" class="btn btn-primary px-4">Reload</button>
        </div>
        <div class="col-auto">
          <button @click="handleSave" class="btn btn-primary px-4">Save Test</button>
        </div>
      </div>
    </div>
    <!-- 预览 区域 -->
    <div class="preview">
      <div ref="style-container"></div>
      <div ref="document_container"></div>
    </div>
  </main>
</template>
