<script setup lang="ts">
import { ref, computed } from 'vue'

interface Document {
  id: number
  name: string
  date: string
  size: string
}

const documents = ref<Document[]>([
  { id: 1, name: 'Project Proposal', date: '2024-04-15', size: '2.4 MB' },
  { id: 2, name: 'Q1 Report', date: '2024-04-10', size: '1.8 MB' },
  { id: 3, name: 'Meeting Notes', date: '2024-04-08', size: '0.5 MB' },
  { id: 4, name: 'Budget Analysis', date: '2024-04-05', size: '3.2 MB' },
])

const selectedDocumentId = ref<number>(1)
const documentContent = ref<string>('')
const showSaveMessage = ref(false)

const selectedDocument = computed(() => {
  return documents.value.find((doc) => doc.id === selectedDocumentId.value)
})

const selectDocument = (id: number) => {
  selectedDocumentId.value = id
  documentContent.value = ''
}

const saveDocument = () => {
  // Demo only - show temporary message
  showSaveMessage.value = true
  setTimeout(() => {
    showSaveMessage.value = false
  }, 2000)
}

const createNewDocument = () => {
  const newDoc: Document = {
    id: documents.value.length + 1,
    name: 'New Document',
    date: new Date().toISOString().split('T')[0],
    size: '0 MB',
  }
  documents.value.push(newDoc)
  selectedDocumentId.value = newDoc.id
}
</script>

<template>
  <div class="dms-container">
    <!-- Header -->
    <header class="dms-header">
      <h1 class="dms-title">📄 Document Management System</h1>
      <button class="btn btn-primary" @click="createNewDocument">+ New Document</button>
    </header>

    <div class="dms-content">
      <!-- Sidebar -->
      <aside class="dms-sidebar">
        <div class="sidebar-header">
          <h2>Documents</h2>
        </div>
        <div class="documents-list">
          <button
            v-for="doc in documents"
            :key="doc.id"
            :class="['document-item', { active: selectedDocumentId === doc.id }]"
            @click="selectDocument(doc.id)"
          >
            <div class="document-icon">📋</div>
            <div class="document-info">
              <div class="document-name">{{ doc.name }}</div>
              <div class="document-meta">{{ doc.date }}</div>
            </div>
          </button>
        </div>
      </aside>

      <!-- Main Editor -->
      <main class="dms-editor">
        <div v-if="selectedDocument" class="editor-container">
          <!-- Document Header -->
          <div class="document-header">
            <div class="document-details">
              <h2 class="document-title">{{ selectedDocument.name }}</h2>
              <div class="document-stats">
                <span class="stat">📅 {{ selectedDocument.date }}</span>
                <span class="stat">💾 {{ selectedDocument.size }}</span>
              </div>
            </div>
            <button class="btn btn-success" @click="saveDocument">Save</button>
          </div>

          <!-- Textarea for Rich Text Content -->
          <textarea
            v-model="documentContent"
            class="rich-text-area"
            placeholder="Start typing your document content here... (Replace this textarea with your rich text editor)"
          ></textarea>

          <!-- Editor Footer -->
          <div class="editor-footer">
            <div class="char-count">{{ documentContent.length }} characters</div>
          </div>
        </div>
        <div v-else class="empty-state">
          <div class="empty-icon">📭</div>
          <p>No document selected</p>
        </div>
      </main>
    </div>

    <!-- Save Notification -->
    <transition name="fade">
      <div v-if="showSaveMessage" class="save-notification">
        ✓ Demo mode - Changes are not saved
      </div>
    </transition>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.dms-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background: #f5f5f5;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu,
    Cantarell, sans-serif;
}

/* Header */
.dms-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.dms-title {
  margin: 0;
  font-size: 28px;
  font-weight: 600;
}

/* Buttons */
.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.btn-primary {
  background-color: white;
  color: #667eea;
}

.btn-primary:hover {
  background-color: #f0f0f0;
  transform: translateY(-2px);
}

.btn-success {
  background-color: #48bb78;
  color: white;
}

.btn-success:hover {
  background-color: #38a169;
  transform: translateY(-2px);
}

/* Content Area */
.dms-content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

/* Sidebar */
.dms-sidebar {
  width: 280px;
  background: white;
  border-right: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.sidebar-header {
  padding: 20px;
  border-bottom: 1px solid #e0e0e0;
}

.sidebar-header h2 {
  margin: 0;
  font-size: 16px;
  color: #333;
}

.documents-list {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.document-item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  margin-bottom: 8px;
  background: transparent;
  border: 1px solid transparent;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: left;
}

.document-item:hover {
  background-color: #f5f5f5;
}

.document-item.active {
  background-color: #e8eaf6;
  border-color: #667eea;
}

.document-icon {
  font-size: 24px;
  flex-shrink: 0;
}

.document-info {
  flex: 1;
  min-width: 0;
}

.document-name {
  font-size: 14px;
  font-weight: 500;
  color: #333;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.document-meta {
  font-size: 12px;
  color: #999;
  margin-top: 2px;
}

/* Editor */
.dms-editor {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: white;
  overflow: hidden;
}

.editor-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 30px;
  gap: 20px;
}

.document-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 20px;
}

.document-details {
  flex: 1;
}

.document-title {
  margin: 0 0 12px 0;
  font-size: 24px;
  color: #333;
}

.document-stats {
  display: flex;
  gap: 20px;
  font-size: 13px;
  color: #666;
}

.stat {
  display: flex;
  align-items: center;
  gap: 6px;
}

.rich-text-area {
  flex: 1;
  padding: 16px;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
  font-size: 14px;
  resize: none;
  outline: none;
  transition: border-color 0.3s ease;
}

.rich-text-area:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.editor-footer {
  display: flex;
  justify-content: flex-end;
  padding-top: 12px;
  border-top: 1px solid #e0e0e0;
  font-size: 12px;
  color: #999;
}

.char-count {
  font-size: 13px;
  color: #666;
}

/* Empty State */
.empty-state {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: #999;
}

.empty-icon {
  font-size: 64px;
  margin-bottom: 20px;
  opacity: 0.5;
}

/* Scrollbar Styling */
.documents-list::-webkit-scrollbar {
  width: 6px;
}

.documents-list::-webkit-scrollbar-track {
  background: transparent;
}

.documents-list::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 3px;
}

.documents-list::-webkit-scrollbar-thumb:hover {
  background: #999;
}

.rich-text-area::-webkit-scrollbar {
  width: 8px;
}

.rich-text-area::-webkit-scrollbar-track {
  background: transparent;
}

.rich-text-area::-webkit-scrollbar-thumb {
  background: #d0d0d0;
  border-radius: 4px;
}

.rich-text-area::-webkit-scrollbar-thumb:hover {
  background: #999;
}

/* Save Notification */
.save-notification {
  position: fixed;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #fed7d7;
  color: #c53030;
  padding: 12px 24px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  z-index: 1000;
}

/* Fade Transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
