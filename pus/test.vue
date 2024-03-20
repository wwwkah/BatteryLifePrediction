<template>
  <CodeBlock :code="code" />
</template>

<script lang="ts" setup>
import CodeBlock from "~/components/CodeBlock.vue";

// ページで表示するコードの内容

const code = `import pandas as pd
import matplotlib.pyplot as plt

# -----年次データの方----- #
# Excelファイルを読み込む(年次データの方)
excel_file_path_1 = 'hon-t07.xls'  # Excelファイルのパスを指定してください
sheet_name = 'TL'  # シート名を指定してください

df_1 = pd.read_excel(excel_file_path_1, sheet_name=sheet_name)

# 行の抽出：48行目から80行目までを選択
selected_rows = df_1.iloc[list(range(46, 79))]

# 列の抽出：A列、B列(1、2列目)を選択
selected_columns = df_1.iloc[:, [0, 1]]

# 選択した行と列でデータを抽出(抽出後のデータフレーム名をdf_1とした)
df_1 = selected_rows[selected_columns.columns]

# カラム名の変更
new_column_names = ['year', 'year_ave']
df_1.columns = new_column_names

# 行のインデックス番号を上から付け直す
df_1.reset_index(drop=True, inplace=True)

# 指数表記から実数値に変換する
df_1['year_ave'] = df_1['year_ave'] * (318299 * 12) / 100

# グラフの描画
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(df_1['year'], df_1['year_ave'])

# x軸のラベルを設定
ax.set_xlabel('year')

# y軸のラベルを設定
ax.set_ylabel('yen')

# y軸の表記を調整（指数表記を無効にする）
ax.ticklabel_format(axis='y', style='plain')

# グラフを表示
plt.show()

# -----四半期データの方----- #
# Excelファイルを読み込む
excel_file_path_2 = 'hon-k07.xls'  # Excelファイルのパスを指定してください
sheet_name = 'TL'  # シート名を指定してください

df_2 = pd.read_excel(excel_file_path_2, sheet_name=sheet_name)

# 行の抽出：10行目から42行目までを選択
selected_rows = df_2.iloc[list(range(8, 41))]

# 列の抽出：A列、B列、C列、D列、E列(1、2、3、4、5列目)を選択
selected_columns = df_2.iloc[:, [0, 1, 2, 3, 4]]

# 選択した行と列でデータを抽出(抽出後のデータフレーム名をdf_2とした)
df_2 = selected_rows[selected_columns.columns]

# カラム名の変更
new_column_names = ['year', '1st_Qr', '2nd_Qr', '3rd_Qr', '4th_Qr']
df_2.columns = new_column_names

# 行のインデックス番号を上から付け直す
df_2.reset_index(drop=True, inplace=True)

# 指数表記から実数値に変換する
df_2[df_2.columns.difference(['year'])] *= (318299 * 12)/100

# 結果を格納するためのからのリストを作成
expanded_years = []

# 各年を四半期ごとに展開
for year in df_2['year']:
    for quarter in range(1, 5):
        expanded_years.append(year + (quarter - 1) * 0.25)

# リストを新しいデータフレームに変換
expanded_X = pd.DataFrame({'expanded_year': expanded_years})

# 結果を格納するための空のリストを作成
result_list = []

# 各年のデータを取得してリストに追加
for i in range(1990, 2023):
    year_data = df_2[df_2['year'] == i][['1st_Qr', '2nd_Qr', '3rd_Qr', '4th_Qr']].values.flatten()
    result_list.extend(year_data)

# リストを新しいデータフレームに変換
result_Y = pd.DataFrame({'Result': result_list})

# グラフの描画
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(expanded_X['expanded_year'], result_Y['Result'])

# x軸のラベルを設定
ax.set_xlabel('year')

# y軸のラベルを設定
ax.set_ylabel('yen')

# y軸の表記を調整（指数表記を無効にする）
ax.ticklabel_format(axis='y', style='plain')

# グラフを表示
plt.show()`;
</script>
