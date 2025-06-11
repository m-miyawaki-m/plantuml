はい、Vuex には `mutation` 以外にもいくつかの主要な機能があります。以下に Vuex の構成要素をまとめて解説します。

---

## ✅ Vuex の4大構成要素

| 構成要素          | 説明                      | 呼び出し方                            | 用途           |
| ------------- | ----------------------- | -------------------------------- | ------------ |
| **state**     | アプリ全体で共有するデータ           | `store.state.xxx`                | 値の保持         |
| **getters**   | state から派生した計算値         | `store.getters.xxx`              | 値の加工・取得      |
| **mutations** | state を変更する唯一の方法        | `store.commit('xxx', payload)`   | 同期的な状態更新     |
| **actions**   | 非同期処理や複数mutationを含むロジック | `store.dispatch('xxx', payload)` | API通信など非同期処理 |

---

## 🔸 mutation 以外の存在まとめ

### 1. **state（状態）**

```ts
state: {
  count: 0
}
```

* 単なるデータの置き場所。
* 直接書き換えるのは NG（Vuex のルールに反する）。

---

### 2. **getters（状態の派生値）**

```ts
getters: {
  doubleCount: (state) => state.count * 2
}
```

* `computed` のような役割。
* コンポーネントでは `store.getters.doubleCount` で取得。

---

### 3. **actions（非同期処理）**

```ts
actions: {
  async incrementAsync({ commit }) {
    await delay(1000)
    commit('increment')
  }
}
```

* API 通信や遅延処理などをまとめる。
* コンポーネントでは `store.dispatch('incrementAsync')` で呼び出し。

---

### 4. **modules（モジュール分割）**

```ts
modules: {
  user: userModule,
  panel: panelModule
}
```

* ストアをモジュール単位で分けて管理しやすくする。
* `namespaced: true` を使えば `store.commit('user/setName')` のように名前空間が使える。

---

## ✅ 一覧表（構成要素の役割）

| 機能          | 概要        | 操作方法 | 例                                  |
| ----------- | --------- | ---- | ---------------------------------- |
| `state`     | 状態そのもの    | 読み取り | `store.state.count`                |
| `getters`   | 加工された状態   | 読み取り | `store.getters.doubleCount`        |
| `mutations` | 状態の変更（同期） | 書き込み | `store.commit('increment')`        |
| `actions`   | 非同期処理     | 書き込み | `store.dispatch('incrementAsync')` |
| `modules`   | 状態の分割管理   | 構造整理 | `store.commit('user/setName')`     |

---

## ✅ 結論

* `mutation` 以外にも Vuex には **`state`, `getters`, `actions`, `modules`** があり、それぞれ役割が違います。
* **stateの読み取り → getters経由もOK**
* **stateの書き換え → mutation経由がルール**
* **非同期処理 → action を利用**

---

必要であれば、Option APIに即したVuex構成テンプレートも提示可能です！
