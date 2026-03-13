<template>
  <div class="resume-page">
    <div class="container">
      <h1 class="title">简历上传</h1>
      
      <div class="upload-section">
        <div 
          class="upload-area" 
          :class="{ 'drag-over': isDragOver, 'has-file': uploadedFile }"
          @drop="handleDrop"
          @dragover.prevent="isDragOver = true"
          @dragleave="isDragOver = false"
          @click="triggerFileInput"
        >
          <input
            ref="fileInput"
            type="file"
            accept=".pdf"
            @change="handleFileSelect"
            style="display: none"
          />
          
          <div v-if="!uploadedFile" class="upload-placeholder">
            <svg class="upload-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
              <polyline points="17 8 12 3 7 8"></polyline>
              <line x1="12" y1="3" x2="12" y2="15"></line>
            </svg>
            <p class="upload-text">点击或拖拽PDF文件到这里上传</p>
            <p class="upload-hint">支持 PDF 格式</p>
          </div>
          
          <div v-else class="file-info">
            <svg class="file-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
              <polyline points="14 2 14 8 20 8"></polyline>
            </svg>
            <p class="file-name">{{ uploadedFile.name }}</p>
            <p class="file-size">{{ formatFileSize(uploadedFile.size) }}</p>
            <button @click.stop="removeFile" class="remove-btn">移除</button>
          </div>
        </div>
      </div>

      <div v-if="uploadedFile" class="preview-section">
        <div class="preview-header">
          <h2>简历预览</h2>
          <button @click="downloadFile" class="download-btn">下载</button>
        </div>
        <div class="pdf-container">
          <iframe 
            :src="pdfUrl" 
            class="pdf-viewer"
            frameborder="0"
            type="application/pdf"
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const fileInput = ref(null)
const uploadedFile = ref(null)
const pdfUrl = ref('')
const isDragOver = ref(false)

// IndexedDB 存储键名
const DB_NAME = 'ResumeStorage'
const DB_VERSION = 1
const STORE_NAME = 'resumes'

// 初始化 IndexedDB
const initDB = () => {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION)
    
    request.onerror = () => reject(request.error)
    request.onsuccess = () => resolve(request.result)
    
    request.onupgradeneeded = (event) => {
      const db = event.target.result
      if (!db.objectStoreNames.contains(STORE_NAME)) {
        db.createObjectStore(STORE_NAME)
      }
    }
  })
}

// 保存文件到 IndexedDB
const saveFileToDB = async (file) => {
  try {
    // 使用 FileReader 将文件转换为 base64，确保数据完整性
    const base64 = await new Promise((resolve, reject) => {
      const reader = new FileReader()
      reader.onload = () => {
        // 移除 data:application/pdf;base64, 前缀
        const base64String = reader.result.split(',')[1]
        resolve(base64String)
      }
      reader.onerror = reject
      reader.readAsDataURL(file)
    })
    
    const db = await initDB()
    
    const fileData = {
      name: file.name,
      size: file.size,
      type: file.type,
      base64: base64, // 使用 base64 存储，确保数据完整性
      lastModified: file.lastModified || Date.now()
    }
    
    return new Promise((resolve, reject) => {
      const transaction = db.transaction([STORE_NAME], 'readwrite')
      const store = transaction.objectStore(STORE_NAME)
      
      const request = store.put(fileData, 'current')
      
      request.onsuccess = () => {
        console.log('文件已保存到 IndexedDB (base64)')
      }
      
      request.onerror = () => {
        console.error('保存文件失败:', request.error)
        reject(request.error)
      }
      
      // 等待事务完成
      transaction.oncomplete = () => {
        console.log('保存事务完成')
        resolve(true)
      }
      
      transaction.onerror = () => {
        console.error('保存事务失败:', transaction.error)
        reject(transaction.error)
      }
    })
  } catch (error) {
    console.error('保存文件失败:', error)
    return false
  }
}

// 从 IndexedDB 加载文件
const loadFileFromDB = async () => {
  try {
    const db = await initDB()
    const transaction = db.transaction([STORE_NAME], 'readonly')
    const store = transaction.objectStore(STORE_NAME)
    const request = store.get('current')
    
    return new Promise((resolve, reject) => {
      request.onsuccess = () => {
        const fileData = request.result
        console.log('从 IndexedDB 读取数据:', fileData ? '找到数据' : '未找到数据')
        
        if (fileData) {
          try {
            let file
            
            // 优先使用 base64（新格式）
            if (fileData.base64) {
              // 从 base64 恢复文件
              const binaryString = atob(fileData.base64)
              const bytes = new Uint8Array(binaryString.length)
              for (let i = 0; i < binaryString.length; i++) {
                bytes[i] = binaryString.charCodeAt(i)
              }
              const blob = new Blob([bytes], { type: fileData.type || 'application/pdf' })
              file = new File(
                [blob],
                fileData.name,
                {
                  type: fileData.type || 'application/pdf',
                  lastModified: fileData.lastModified || Date.now()
                }
              )
              console.log('成功恢复文件 (base64):', file.name, '大小:', file.size)
            } 
            // 兼容旧格式（ArrayBuffer）
            else if (fileData.data) {
              let arrayBuffer = fileData.data
              
              if (arrayBuffer instanceof ArrayBuffer) {
                const blob = new Blob([arrayBuffer], { type: fileData.type || 'application/pdf' })
                file = new File(
                  [blob],
                  fileData.name,
                  {
                    type: fileData.type || 'application/pdf',
                    lastModified: fileData.lastModified || Date.now()
                  }
                )
                console.log('成功恢复文件 (ArrayBuffer):', file.name, '大小:', file.size)
              } else if (arrayBuffer instanceof Uint8Array) {
                const blob = new Blob([arrayBuffer.buffer], { type: fileData.type || 'application/pdf' })
                file = new File(
                  [blob],
                  fileData.name,
                  {
                    type: fileData.type || 'application/pdf',
                    lastModified: fileData.lastModified || Date.now()
                  }
                )
                console.log('成功恢复文件 (Uint8Array):', file.name, '大小:', file.size)
              } else {
                console.error('不支持的数据格式')
                resolve(null)
                return
              }
            } else {
              console.error('文件数据格式不正确')
              resolve(null)
              return
            }
            
            resolve(file)
          } catch (error) {
            console.error('转换文件对象失败:', error)
            resolve(null)
          }
        } else {
          resolve(null)
        }
      }
      request.onerror = () => {
        console.error('读取 IndexedDB 失败:', request.error)
        reject(request.error)
      }
    })
  } catch (error) {
    console.error('加载文件失败:', error)
    return null
  }
}

// 从 IndexedDB 删除文件
const deleteFileFromDB = async () => {
  try {
    const db = await initDB()
    const transaction = db.transaction([STORE_NAME], 'readwrite')
    const store = transaction.objectStore(STORE_NAME)
    store.delete('current')
    return true
  } catch (error) {
    console.error('删除文件失败:', error)
    return false
  }
}

// 组件挂载时加载已保存的文件
onMounted(async () => {
  console.log('开始加载已保存的文件...')
  const savedFile = await loadFileFromDB()
  if (savedFile) {
    console.log('找到已保存的文件:', savedFile.name)
    await processFile(savedFile, false) // false 表示不保存，因为已经存在
  } else {
    console.log('没有找到已保存的文件')
  }
})

const triggerFileInput = () => {
  if (!uploadedFile.value) {
    fileInput.value?.click()
  }
}

const handleFileSelect = async (event) => {
  const file = event.target.files[0]
  if (file && file.type === 'application/pdf') {
    console.log('选择文件:', file.name)
    await processFile(file, true)
  } else {
    alert('请上传PDF格式的文件')
  }
}

const handleDrop = async (event) => {
  event.preventDefault()
  isDragOver.value = false
  
  const file = event.dataTransfer.files[0]
  if (file && file.type === 'application/pdf') {
    console.log('拖拽文件:', file.name)
    await processFile(file, true)
  } else {
    alert('请上传PDF格式的文件')
  }
}

const processFile = async (file, shouldSave = true) => {
  uploadedFile.value = file
  pdfUrl.value = URL.createObjectURL(file)
  // 保存到 IndexedDB（如果是新上传的文件）
  if (shouldSave) {
    try {
      await saveFileToDB(file)
      console.log('文件处理完成并已保存')
    } catch (error) {
      console.error('保存文件时出错:', error)
    }
  } else {
    console.log('文件已恢复，无需重新保存')
  }
}

const removeFile = async () => {
  if (pdfUrl.value) {
    URL.revokeObjectURL(pdfUrl.value)
  }
  uploadedFile.value = null
  pdfUrl.value = ''
  if (fileInput.value) {
    fileInput.value.value = ''
  }
  // 从 IndexedDB 删除
  await deleteFileFromDB()
}

const formatFileSize = (bytes) => {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i]
}

const downloadFile = () => {
  if (uploadedFile.value) {
    const link = document.createElement('a')
    link.href = pdfUrl.value
    link.download = uploadedFile.value.name
    link.click()
  }
}
</script>

<style scoped>
.resume-page {
  min-height: 100vh;
  padding: 40px 20px;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.title {
  text-align: center;
  color: white;
  font-size: 2.5rem;
  margin-bottom: 40px;
  font-weight: 600;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.upload-section {
  margin-bottom: 40px;
}

.upload-area {
  background: white;
  border: 3px dashed #ddd;
  border-radius: 16px;
  padding: 60px 20px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.upload-area:hover {
  border-color: #667eea;
  background: #f8f9ff;
  transform: translateY(-2px);
  box-shadow: 0 6px 30px rgba(102, 126, 234, 0.2);
}

.upload-area.drag-over {
  border-color: #667eea;
  background: #f0f4ff;
  transform: scale(1.02);
}

.upload-area.has-file {
  border-color: #10b981;
  background: #f0fdf4;
  cursor: default;
}

.upload-placeholder {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.upload-icon {
  width: 64px;
  height: 64px;
  color: #667eea;
  stroke-width: 2;
}

.upload-text {
  font-size: 1.25rem;
  color: #333;
  font-weight: 500;
}

.upload-hint {
  font-size: 0.9rem;
  color: #666;
}

.file-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.file-icon {
  width: 48px;
  height: 48px;
  color: #10b981;
  stroke-width: 2;
}

.file-name {
  font-size: 1.1rem;
  color: #333;
  font-weight: 500;
  word-break: break-all;
  max-width: 100%;
}

.file-size {
  font-size: 0.9rem;
  color: #666;
}

.remove-btn {
  margin-top: 8px;
  padding: 8px 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s ease;
}

.remove-btn:hover {
  background: #dc2626;
  transform: translateY(-1px);
}

.preview-section {
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.preview-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 2px solid #f0f0f0;
}

.preview-header h2 {
  color: #333;
  font-size: 1.5rem;
}

.download-btn {
  padding: 10px 24px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.95rem;
  font-weight: 500;
  transition: all 0.2s ease;
}

.download-btn:hover {
  background: #5568d3;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.pdf-container {
  width: 100%;
  height: 800px;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid #e5e7eb;
  background: #f9fafb;
}

.pdf-viewer {
  width: 100%;
  height: 100%;
  border: none;
}

@media (max-width: 768px) {
  .title {
    font-size: 2rem;
  }
  
  .upload-area {
    padding: 40px 20px;
  }
  
  .pdf-container {
    height: 600px;
  }
  
  .preview-header {
    flex-direction: column;
    gap: 16px;
    align-items: flex-start;
  }
}
</style>

