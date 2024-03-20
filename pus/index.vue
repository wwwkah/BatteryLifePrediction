<template>
  <h1 class="heading-text">トップページ</h1>

  <!-- ListItem コンポーネントに props として listItem を渡す -->
  <ListItem :links="listItems" />
</template>

<script setup>
import ListItem from '@/components/ListItem.vue' // コンポーネントのインポート

// ページ側でのプロップスの設定
const listItems = [
  { text: 'Yahoo', url: 'https://www.yahoo.co.jp' },
  { text: 'Google', url: 'https://www.google.com' },
  { text: 'Microsoft', url: 'https://www.microsoft.com' },
  { text: 'Apple', url: 'https://www.apple.com' },
  { text: 'Amazon', url: 'https://www.amazon.com' }
  // 他のリンクも同様に追加
]

/*あるべきプロップスの渡し方*/
/*
const LinkLists = [
  { "id": "nominal-gdp-of-japan", "name": "GDPの推移(日本)" },
  { "id": "nominal-gdp-of-japan", "name": "GDPの推移(日本)" },
  { "id": "nominal-gdp-of-japan", "name": "GDPの推移(日本)" },
  { "id": "nominal-gdp-of-japan", "name": "GDPの推移(日本)" }
]
*/
</script>
