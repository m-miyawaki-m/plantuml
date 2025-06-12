Vue（特に Vue 3 のオプション API）における **`created`** と **`mounted`** の違いは、**コンポーネントのライフサイクル（生成から表示までの流れ）**における呼ばれる**タイミングの差**にあります。

---

## ✅ created vs mounted の違い（Vue 3 オプションAPI）

| 比較項目              | `created`                | `mounted`                       |
| ----------------- | ------------------------ | ------------------------------- |
| 発生タイミング           | インスタンス生成後、DOM 未挿入の状態で    | DOM にコンポーネントが**描画完了**した直後       |
| DOM へのアクセス        | ❌ できない（DOM未生成）           | ✅ 可能（`$el`などでアクセス可）             |
| API 呼び出し          | ✅ 可能（非同期OK）              | ✅ 可能（ただし DOMが必要なものはこちら）         |
| 主な用途              | - 初期データの取得<br>- Vuex 初期化 | - DOM 操作（focus, chart描画など）      |
| Vue 3 setup()との関係 | `setup()` の後に実行          | `setup()` → DOM描画 → `mounted()` |

---

## 🧪 イメージ図（ライフサイクル順）

```text
beforeCreate
↓
created      ← ★Vueインスタンス生成・props/data 使用OK
↓
beforeMount
↓
mounted      ← ★DOM描画完了（templateがHTML上に配置済み）
↓
beforeUpdate
↓
updated
↓
beforeUnmount
↓
unmounted
```

---

## 🔧 使用例（Option API）

```ts
export default defineComponent({
  name: 'SampleComponent',

  data() {
    return {
      message: 'Hello',
    };
  },

  created() {
    console.log('✅ created: API呼び出しやstore初期化に最適');
    // 例：this.fetchInitialData()
  },

  mounted() {
    console.log('✅ mounted: DOMにアクセス可能');
    // 例：this.$refs.input.focus()
  },
});
```

---

## 📌 まとめ：使い分けの目安

| やりたいこと            | 使用するライフサイクルフック          |
| ----------------- | ----------------------- |
| 初期データ取得（API）      | `created` または `setup()` |
| storeへの初期commit   | `created`               |
| DOM操作（focusなど）    | `mounted`               |
| グラフ描画（Chart.jsなど） | `mounted`               |

---

必要であれば、**ライフサイクル全体を図として表した PlantUML** もご用意します。作成しましょうか？
