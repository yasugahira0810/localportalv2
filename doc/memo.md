- 単一ファイルコンポーネントのscriptブロックにおいては、exportが必須。
- コンポーネントにデータを持たせる場合、dataオプションは関数にしてreturnする必要がある。各インスタンスが返されるデータオブジェクトの独立したコピーを保持できるため。（[参考1]([https://jp.vuejs.org/v2/guide/components.html#data-%E3%81%AF%E9%96%A2%E6%95%B0%E3%81%A7%E3%81%AA%E3%81%91%E3%82%8C%E3%81%B0%E3%81%AA%E3%82%8A%E3%81%BE%E3%81%9B%E3%82%93](https://jp.vuejs.org/v2/guide/components.html#data-は関数でなければなりません)), [参考2](https://qiita.com/shosho/items/115b371736207078289c)）

```javascript
<script>
export default {
  data() {
    return {
      siteName: "",
      siteURL: ""
    };
```



### Vue Loader

- 単一ファイルコンポーネントをJavaScriptに変換するwebpackのloader
  - webpack: JavaScriptファイルをまとめる高機能なモジュールバンドラー

