Vue 的に「storeを監視する」とは、Vue コンポーネント内で `watch()` を使って Vuex の `state` を監視する処理を意味します。具体的には以下のように表現します：

---

## ✅ Vue 3（Composition API）での store 監視の書き方

```ts
import { watch } from 'vue'
import { useStore } from 'vuex'

export default {
  setup() {
    const store = useStore()

    // Vuex の state.ui.mainTab を監視
    watch(
      () => store.state.ui.mainTab,     // 監視対象
      (newVal, oldVal) => {             // コールバック
        console.log('mainTab changed:', newVal)
        updateMainAreaTab(newVal)       // メソッドで反映
      }
    )

    function updateMainAreaTab(tab: number) {
      // 例: props 経由で MainArea に渡すための処理
    }

    return {}
  }
}
```

---

## ✅ Options API の場合

```ts
export default {
  computed: {
    mainTab() {
      return this.$store.state.ui.mainTab
    }
  },
  watch: {
    mainTab(newVal) {
      this.updateMainAreaTab(newVal)
    }
  },
  methods: {
    updateMainAreaTab(tab) {
      // エリアの表示切替処理
    }
  }
}
```

---

## ✅ 設計書での表現にするなら：

シーケンス図の「`group watch: store.state.ui.mainTab`」は、**Vue コンポーネント内で `watch(() => store.state.ui.mainTab)` を使って監視している**、という意味です。

今後このような表記を使う場合は、以下のような注釈をドキュメントに加えてもよいでしょう：

---

### 💬 表記ガイド補足（ドキュメントに追記可能）

> `group watch: store.state.X` の表現は、Vue コンポーネントにおいて Composition API の `watch()` を用いて `Vuex Store` の `state` を監視し、副作用（props 更新や UI 切り替え）を起こす処理を表します。

---

ご希望であれば、上記の表現を既存のテンプレートドキュメントに**注釈（ガイド）として追記**できますが、どうしますか？

ご質問の「モーダル表示ボタン押下 → ストアの値更新 → ストア監視 → モーダルに表示するデータ取得」という流れのタイミング制御は、**Vue のリアクティブ機構 + `watch()` の非同期性**に依存します。以下で Vue 的にどのように制御・実装されるかを説明します。

---

## ✅ 流れの各ステップと Vue 的な制御

| ステップ           | 説明                                                                 | Vue の処理                |
| -------------- | ------------------------------------------------------------------ | ---------------------- |
| ① モーダルボタン押下    | 例: `@click="openEdit(commentId)"`                                  | メソッド内で `store.commit`  |
| ② store の値更新   | 例: `store.commit('comment/setEditTarget', id)`                     | `state.editTarget` に反映 |
| ③ watch が発火    | `watch(() => store.state.comment.editTarget, ...)`                 | 新しい id に反応して処理開始       |
| ④ モーダルに渡すデータ取得 | `store.getters['comment/current']` や `store.dispatch('fetchById')` | watch 内で非同期も可          |
| ⑤ モーダル表示       | `v-model` や `v-if` による表示切り替え                                       | props 経由 or ローカル状態で制御  |

---

## 🔍 注意点：非同期データ取得タイミングの制御方法

### 方法A：getter ですでに保持しているデータを使う

```ts
watch(() => store.state.comment.editTarget, (id) => {
  comment.value = store.getters['comment/current']
  showModal.value = true
})
```

### 方法B：watch 内で API を再取得

```ts
watch(() => store.state.comment.editTarget, async (id) => {
  await store.dispatch('comment/fetchById', id)
  comment.value = store.state.comment.current
  showModal.value = true
})
```

> ✅ この方法だと「取得完了してから表示」と制御できる。

---

## ✅ シーケンス図での記述対応（テンプレートとして追記予定）

```plantuml
== 編集ボタン押下 ==
User -> Card : 編集クリック
Card -> Store : commit('comment/setEditTarget', commentId)

== store を監視 ==
group watch: store.state.comment.editTarget が変更されたら
  Page -> Store : dispatch('comment/fetchById')
  Store -> Page : 編集対象データ取得完了
  Page -> Modal : props.comment = store.state.comment.current
  Page -> Modal : 表示（v-model = true）
end
```

---

## ✅ 補足：watch の発火タイミングは同期？非同期？

* `watch()` 自体の発火は **同期**
* ただし、Vue の更新（DOM反映）は **次の tick に非同期で反映**
* `await nextTick()` を使えば「描画後に処理」も制御可能

---

## ✅ まとめ

Vue の設計においては：

* データ取得完了 → 表示、の順に制御したければ `watch` 内で `dispatch → then → モーダル表示` が◎
* getter で済むなら軽く即時反映可能
* `watch + dispatch + set v-model` が最も一般的なパターン

---

ご希望であれば、この「編集ボタン → store更新 → watch → データ取得 → モーダル表示」の**完全版テンプレート図**を既存テンプレート集に追記いたします。追加しますか？
