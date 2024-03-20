<script setup>
/**
 * リストアイテムを表示するコンポーネント
 *
 * このコンポーネントにソースコードを渡す際はテンプレートリテラルを使用すること
 *   <ListItem :item="`${itemName}`" />
 */

// プロップスを定義
const props = defineProps(['links'])

const data = reactive({
  isHovered: false
})

const handleMouseOver = () => {
  data.isHovered = true
}

const handleMouseOut = () => {
  data.isHovered = false
}
</script>

<template>
  <ul>
    <li
      v-for="(link, index) in links"
      :key="index"
      @mouseover="handleMouseOver"
      @mouseout="handleMouseOut"
    >
      <a :href="link.url" target="_blank" :class="{ 'custom-link-hover': isHovered }">{{
        link.text
      }}</a>
    </li>
  </ul>
</template>

<style scoped>
li {
  height: 48px;
  width: 100%;
  background-color: white;
  margin-bottom: 8px;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s;
  padding: 8px;
  border-bottom: 1px solid #ccc;
}

li:hover {
  background-color: var(--color-sub-background);
  color: var(--color-highlight-text);
  a {
    color: var(--color-highlight-text);
  }
}

a {
  color: var(--color-paragraph-text);
  text-decoration: none; /* リンクの下線を無効にする */
}
</style>
