<template>
  <div class="grid grid-cols-12 gap-8">
      <div class="col-span-full lg:col-span-6">
          <div class="card">
              <div class="font-semibold text-xl mb-4">ファイルアプロード</div>
              <object
                v-if="pdfResource"
                :data="pdfResource"
                width="100%"
                height="100%">
                <p>PDF を表示できない場合</p>
              </object>
          </div>
      </div>
      <div class="col-span-full lg:col-span-6">
          <div class="card">
              <div class="font-semibold text-xl mb-4">ファイルリスト</div>
              <form>
              <div
                v-for="field in fieldResource.fields"
                :key="field.name"
                class="field"
              >
                <label for="multiple-ac-1" :for="field.name" class="font-bold mb-2 block">{{ field.name }}</label>
                <AutoComplete :name="field.name" :value="field.value" multiple fluid :suggestions="items" @complete="search" />
              </div>
            </form>
          </div>
      </div>
  </div>
</template>
  
<script setup>
  import { ref } from 'vue'
  import axios from 'axios'
  import sampleData from '@/assets/data/sampleData.json'
  
  const pdfLoaded = ref(false)
  const pdfResource = ref('')
  const fieldResource = ref({})
  const pdfInputRef = ref(null) 
  
  function selectPdfFile() {
    pdfInputRef.value.click() 
  }
  
  async function handleFileChange(event) {
    const file = event.target.files[0]
    if (!file) return
  
    // 1) 在 object 标签显示
    pdfResource.value = URL.createObjectURL(file)
    pdfLoaded.value = true
  
    // 2) 通过 axios 上传到后端 /api/upload
    try {
      const formData = new FormData()
      formData.append('file', file)
  
      const res = await axios.post('http://localhost:5001/api/upload', formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
      console.log('Upload success:', res.data)
      // 后端可能返回识别后的结果或图片列表
      // 你可以根据后端返回数据进行后续处理
    } catch (err) {
      console.error('Upload failed:', err)
    }
  }
  
  // 点击“PDF分析”按钮
  function loadFieldResource() {
    fieldResource.value = sampleData
  }
</script>
