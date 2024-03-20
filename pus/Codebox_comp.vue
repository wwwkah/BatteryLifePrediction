<script lang="ts" setup>
import Prism from 'prismjs'
import 'prismjs/themes/prism-tomorrow.css'
import 'prismjs/components/prism-python'
import { defineProps, ref, computed, Ref } from 'vue'

// プロップスを定義
const props = defineProps({
  code: {
    type: String,
    required: true
  }
})

// 状態を定義
const isOpen: Ref<boolean> = ref(false)
const isCopySuccess: Ref<boolean> = ref(false)

// 折りたたまれたコードを計算
const foldedCode = computed(() => {
  const lines = props.code.split('\n')
  if (!isOpen.value && lines.length > 5) {
    return lines.slice(0, 5).join('\n')
  }
  return props.code
})

// ハイライトされたコードを計算
const highlightedCode = computed(() => {
  return Prism.highlight(foldedCode.value, Prism.languages.python, 'python')
})

// クリップボードへのコピー
const copyCode = async () => {
  try {
    await navigator.clipboard.writeText(props.code)
    isCopySuccess.value = true
    console.log('コピーが成功しました')
    setTimeout(() => {
      hideCopySuccessMessage()
    }, 1000)
  } catch (error) {
    console.error('コピーに失敗しました:', error)
  }
}

// メッセージを非表示にする
const hideCopySuccessMessage = () => {
  isCopySuccess.value = false
}
</script>

<template>
  <div class="back">
    <div class="button-container">
      <!-- ボタン：折りたたみ/展開のトグル -->
      <button @click="isOpen = !isOpen">
        {{ isOpen ? '折りたたむ' : '続きを見る' }}
      </button>
    </div>

    <!-- プリフォーマットされたコード表示エリア -->
    <div class="codeback">
      <!-- ボタン：コードのクリップボードへのコピー -->
      <div class="copy-container">
        <v-btn @click="copyCode" class="copy-button">
          <v-icon class="copy-icon">mdi-content-copy</v-icon>
          {{ isCopySuccess ? 'Success!' : 'コピー' }}
        </v-btn>
      </div>
      <pre v-html="highlightedCode"></pre>
    </div>
    <!-- 続きを見るボタン -->
    <div class="show-more-button" @click="isOpen = !isOpen">
      {{ isOpen ? '折りたたむ' : '続きを見る' }}
    </div>

    <!-- コピー成功メッセージ -->
  </div>
  <div v-if="isCopySuccess" class="copy-success-message">コピーが成功しました</div>
</template>

<style scoped>
.sample-component {
  font-size: 1rem;
  font-weight: bold;
  text-align: left;
  width: 50%;
  position: relative;
  margin-right: 10px;
  padding: 10px;
}
.back {
  background: #9f1313;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.button-container {
  bottom: 0;

  transform: translateX(-50%);
  text-align: center;
  font: red;
}
.back_u {
  background: #9f1313;
  margin-bottom: 100px;
}
.codeback {
  background: rgb(234, 233, 229);
  font-family: SFMono-Regular, Menlo, Consolas, 'PT Mono', 'Liberation Mono', Courier, monospace;
  font-size: 85%;
  padding: 34px 16px 32px 32px;
  min-height: 1em;
  position: relative;
  width: 80%;
  max-height: 400px; /* 適切な値を設定 */
  margin: 0 auto;
  overflow: auto;
}

button {
  margin-bottom: 10px;
}

.copy-container {
  position: absolute;
  top: 10px;
  right: 10px;
}

.copy-button {
  background-color: transparent;
  box-shadow: none; /* 影を無効にするためのプロパティ */
  border-radius: 0; /* 丸みをなくすためのプロパティ */
}

.copy-icon {
  margin-right: 4px;
}

.copy-success-message {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #4caf50;
  color: white;
  text-align: center;
  padding: 10px;
  animation: slideIn 0.5s ease-out;
}
.message-box {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 1000px;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  display: none; /* initially hidden */
}

@keyframes slideIn {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}
.show-more-button {
  text-align: center;
  background: #4caf50;
  color: white;
  padding: 10px;
  cursor: pointer;
  position: relative;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1; /* codebackの上に表示するための設定 */
}
.show-more-button:hover {
  background: #45a049;
}
</style>
