サクラエディタ カスタム版  

**Introduction**  

日々使っていて「こうだったらいいな」を入れ込んでいます.  
また、いろんなものを試す場としても使うつもりです.  

MacTypeと併用していて画面崩れがひどかったので対応をしました.  
大抵, 前の描画が残ってしまって見苦しい状態になることが多いです.  
同じような状態に陥っている人には効果があるかもしれませんが重いかもしれません.  
いろいろとカオスなので一時しのぎですが…  

<br>

**Download**  

変更内容はコミットログを参照してください.  

 ・[sakura-mix-2.8.zip](http://mimix.sakura.ne.jp/release/sakura-mix-2.8.zip) (918KB)  
 ・[sakura-mix-2.7.zip](http://mimix.sakura.ne.jp/release/sakura-mix-2.7.zip) (917KB)  

<br>

**Build environment**  
+ 2.2.0.1をベースに[リポジトリ](http://svn.code.sf.net/p/sakura-editor/code/sakura/trunk2)の追っかけ. ベースリビジョンからのマージ情報は[こちら](https://github.com/mimix33/sakura2201c/blob/master/changes_from_r4011.txt). あと、[パッチ](https://sourceforge.net/p/sakura-editor/patchunicode/)のマージ  
+ MSVC2017でビルド  
+ Boostを使用  
+ 挙動の制御 (共有フラグ)としてレジストリを使用しています  

<br>

**Changed**  
+ いくつかの設定をレジストリで変更できます. キーがなくても問題ありません.  
  レジストリの詳細は `my_config.h` を参照してください.  

+ ファイル系
  - 設定情報の読み書きにレジストリを使用できます (デフォルトはini)  
    レジストリキーがない場合はiniファイルから読み込みます  
    アンインストールする際は `[HKEY_CURRENT_USER\SOFTWARE\sakura.ini]` を削除してください (キー名はプロファイル名で変わりますので注意)  
  - 履歴は別ファイル (sakura.recent.ini)で扱う  
  - カラー設定のインポートはカラー情報だけを適用させる  
  - 履歴 (検索, 置換, Grep)の値を少なめに変更  
  - 起動時に存在しない履歴を削除する  
  - 多重オープンの許可 (Shiftを押しながらファイルのドロップ)  

+ 操作, 編集系
  - キャレットのサイズを変更可能に  
  - 垂直, 水平スクロールの挙動を変更, メモ帳の挙動と同じにする  
  - 検索, ジャンプなどのカーソルが大きく移動する処理ではジャンプ先のカーソル行をセンタリングする  
  - タブ入力文字の切り替え機能 (`S_ChangeTabWidth` マクロを修正, 負の値を設定するとタブと空白を相互に切り替えます)  

+ 表示系
  - EOFのみの行 (起動時とか)にも行番号を表示  
  - 行を中央ぞろえにする (通常, 上揃えになっていて行の間隔が下に付加されている)  
  - 半角空白文字を `･` で描画, NBSPも半角空白として `×` で表示  
  - タブ文字を線のみで描画  
  - コメント行の背景カラーを改行以降もその色で描画  
  - 空白, タブ, 改行, EOFのカラーは現在のテキストカラーから自動で設定  
  - 選択範囲カラーは元のテキストカラーをそのまま使用する (Text:0%, Back:100%)  
  - 太字装飾の文字列を選択したときに選択範囲カラーの装飾の影響を受けないように修正  
  - カーソル行アンダーラインを行番号から引っ張る  

+ UI系
  - タブと編集ウィンドウのスタイル調整  
  - リソース (ダイアログ)のフォントを `9, "ＭＳ Ｐゴシック"` から `9, "MS Shell Dlg"` へ変更  
  - タブ名のカラーを変更 (変更, キーマクロ記録中)  
  - タブをダブルクリックで閉じる  
  - 選択タブのアクティブ化をマウス押下時に行う (通常, マウス押上時にアクティブにするのでワンテンポ遅く感じる)  
  - Grepフォルダの指定を物理的に4つに増やした (`;` で区切ると履歴管理が面倒…)  
  - Grep「現在編集中のファイルから検索」をチェックした時の状態を保持しないようにする  
    \- 現在編集中からのGrepって「今回だけ！」ってことが多いと思います  
  - 置換ダイアログの置換後テキストに置換前テキストを設定  
  - 正規表現検索のときに正規表現記号をクォート (`$10^` を検索する場合 `\$10\^` にする)  
  - アウトライン解析ダイアログのフォントに設定フォントを使用, ドッキング時に背景カラーを使用しない (コントロール色のまま)  
  - ステータスバーのカスタマイズ  
    \- カーソル移動時のちらつき抑制  
    \- 表示情報の内容を修正  
    \- 左クリックでメニューを表示  
  - ダイレクトジャンプ一覧の表示カラムを選別  

<br>

**Bugfix**  
+ 検索マーク切り替え, インクリメンタルサーチの際に検索ダイアログの「正規表現」が影響を受けないように  
  常時、正規表現で検索しているとコレ結構ストレスたまります  

+ 行番号縦線を行番号の色で描画する  
  行番号縦線はその行に変更があった場合, その行だけ変更色で縦線が引かれてしまうので区切りの線としては微妙だったため

+ 行番号が非表示でブックマークが表示のときブックマークは線で描画する  
  行番号非表示時のブックマーク表示がなかったのでブックマークのカラー設定を無効にしている時と同じように表示する

<br>


(C) 2017 Yu-zuki@mimix.
