## Next.js

- `Link`タグはページ全体のリフレッシュをせず画面遷移ができる。
- 本番環境でのビルドにおいては、Link コンポーネントがブラウザのビューポートに表示されると、Next.js は自動的にリンク先のページのコードをバックグラウンドでプリフェッチする。リンクをクリックするときまでには、目的のページのコードはバックグラウンドで読み込まれ終わっており、ページ遷移はほぼ瞬時に行われる。

#### 静的生成（Static Generation）

- ビルド時 に HTML を生成するプリレンダリング手法です。プリレンダリングされた HTML は各リクエストに対して 再利用 されます。

#### サーバサイドレンダリング（SSR）

- 毎回のリクエストごとに HTML を生成するプリレンダリング手法です。

## Remix

- Next.js と比べると学習コストが低い？

#### Route

- routes フォルダを作成する必要がある
- `app/routes/contacts.\$contactId.tsx`のような形で階層があっても 1 ファイル名で表現できる。  
  → Next.js だとフォルダ階層が深くなる
- `Link`タグは Next.js と同じ機能
- route のファイル名で`_`をつけることでネストされずその画面全体を対象にできる
- SSR なので、何かデータを更新するとサーバーの方で`loader`が走り自動で画面のデータを更新してくれる  
  → useEffect などで再取得する必要がない
- Global Pending UI が簡単にできる  
  → 画面更新中などに画面が薄くなり、他の処理をできなくする  
  → 他の FW で実現しようとするとかなり難しい？
- `routes/_index.tsx`は親ルートになる。`<Outlet />`は不要。
- `debounce`処理が不要  
  → 例えば検索処理が走る時、ユーザーの入力が早ければリクエストを自動でキャンセルしてくれる。ただしユーザーの入力が遅ければ全てリクエストが投げられる。
- `Streaming`を使うと全取得を待たずレンダリングできる？  
  → `Await`タグと使えばいい？

```
import { Await } from "@remix-run/react";

<Suspense fallback={<div>Loading...</div>}>
  <Await resolve={somePromise}>
    {(resolvedValue) => <p>{resolvedValue}</p>}
  </Await>
</Suspense>;
```

## モノレポ

- 権限管理  
  モノレポでは、プロジェクトごとのアクセス制御が難しくなることがあります。
