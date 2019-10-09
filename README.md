簡単！Webカレンダー実装
======================
2019/08/31
※このページは.md形式で書かれています。
このまま見ることもできますが、対応した拡張機能を備えたテキストエディタ、またはブラウザーで見ることを強く推奨します。
※このページに限りませんが、自動改行機能を備えたエディタを使うことは、あなたの精神衛生にとても良いです。

それではどうぞ

CSSで編集できるカレンダーが欲しいぃ
-----------------------------------
9月のコアイベントまでにプロトタイプを実装しなければならない我々chocomona.は、どうしてもカレンダーの最低限の機能をさっさと実装しなくてはならない。
しかーし！googleが出してる _あれ_ じゃあだせぇし、やる気もでねえ。
そもそもうちらはカレンダーをMap化させるのじゃ！！

と、いうことで今回は __fullcalendar__ でWebカレンダーの部分を実装していきます！

----------------------------------------------------------------------------

fullcalendarとは
----------------

- CSSとJSのファイル群。fullcalendarを作業ディレクトリの適切な場所に配置し、任意のHTMLファイル上であーだこーだすると __カレンダーを表示してくれる。__
- 好き勝手にCSSをいじって __自分好みのWebカレンダーを作れる。__
- GoogleAPIを使って、 __グーグルカレンダーの予定を表示させることができる。__
- カレンダーの表示に関する機能が最低限ある。

__CSSの知識だけでルックスは編集できそうです！__

じゃあ、なにがいるのかなぁー…

----------------------------------------------------------------------------

実装に必要なもの
----------------
- 精神面
  - やる気
  - 元気
  - 根気
- 物理面
  - ３万くらいのパソコン
  - ネット環境
  - 電力
  - 時間（３０分）

どれも大事ですよねー？


----------------------------------------------------------------------------


茶番はこの辺にして。。

前提条件
--------

以下の基礎知識が必要ですっ

- HTML
- CSS

はい！これだけ！！（いけそう）

あ、ごめん、バッグエンドは違うんだ。。

そんなことは気にしないー

まずはフロントさん向けに進めるね〜

----------------------------------------------------------------------------

フロント勢（３０分）
====================

下準備
------

- Google垢を持ってない人はつくる。
- GoogleCalendarAPIキーを取得する。
参考：https://edge.sincar.jp/web/google-calendar-api-key/
※APIキーをわかりやすい場所にメモすること。
- GoogleカレンダーIDのメモ
参考：http://www.sukicomi.net/2018/07/google-calendarid.html
- 任意の場所に、作業用フォルダ（作業ディレクトリ/ホームディレクトリ）を作る。今回の例ではWWWとします。

おしまい（おい）

- 次に以下のようなファイル構成を作る。

```
WWW
|- index.html ← 作業用ふぁいる
|
|- fullcalendar ← 例のCSS、JSファイル群を入れるためのフォルダ。
|
|- README.md ← いま見てるふぁいる
```

これだけ。（これだけ）

え？何言ってるかわからない？

だから〜

```
WWWの下（中）に〜
index.html作って〜
fullcalendarフォルダ作って〜
README.md も入れとく〜
```

だけやで。~~（うざい人）~~

 ~~ほんとはね、ローカル開発環境作ってね、ApacheとかPHPとか実行されているサーバ内でね、やって欲しいんだけどね、~~

 時間がないので次行きます。

メイン作業
----------
- 以下のリンクからfullcalendarをダウンロード。（zip形式）


>Download
>You can also download the library manually:
>
>Latest: [fullcalendar-4.3.1.zip](https://github.com/fullcalendar/fullcalendar/releases/download/v4.3.1/fullcalendar-4.3.1.zip) (view all releases)
>
>Once you’ve downloaded it, you can initialize a calendar using script tags and browser globals

リンク元のURL：https://fullcalendar.io/docs/getting-started

- ダウンロードしたファイルを展開
※ファイル展開のフリーソフトといえば[7zip](https://sevenzip.osdn.jp)だよね〜（優しすぎ）
- fullcalendar-4.3.1のフォルダの中のpackagesフォルダの中身だけを全部、さっき作った/WWW/fullcalendarの中にコピー。

おわり（まだよ！）

- 下準備で作ったindex.htmlの中身をこれにして↓（コピペコピペ！）

```html
<!DOCTYPE html>
<html lang="ja" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>MyTestCalendar</title>
    <!-- 
      fullcalendar用CSSファイル
      参照：https://fullcalendar.io/docs/plugin-index
     -->
    <link href='fullcalendar/core/main.css' rel='stylesheet' />
    <link href='fullcalendar/daygrid/main.css' rel='stylesheet' />
    <link href='fullcalendar/timegrid/main.css' rel='stylesheet' />
    <link href='fullcalendar/list/main.css' rel='stylesheet' />
    <!-- <link href='fullcalendar/bootstrap/main.css' rel='stylesheet' /> -->
    <!-- 
      fullcalendar用JSファイル
      参照：https://fullcalendar.io/docs/plugin-index
     -->
    <script src='fullcalendar/core/main.js'></script>
    <script src="fullcalendar/interaction/main.js"></script>
    <script src='fullcalendar/daygrid/main.js'></script>
    <script src='fullcalendar/timegrid/main.js'></script>
    <script src='fullcalendar/list/main.js'></script>
    <!-- <script src='fullcalendar/bootstrap/main.js'></script> -->
    <script src="fullcalendar/google-calendar/main.js"></script>
    <script src="fullcalendar/rrule/main.js"></script>
    <script src="fullcalendar/luxon/main.js"></script>
    <script src='fullcalendar/moment/main.js'></script>
    <script src='fullcalendar/moment-timezone/main.js'></script>
    <!-- fullcalendar のメイン処理（jQuery）-->
    <script>
      document.addEventListener('DOMContentLoaded', function() {
        var calendarEl = document.getElementById('calendar');
        var calendar = new FullCalendar.Calendar(calendarEl, {
          //とりあえずフルでプラグインを入れとく
          plugins: [ 'interaction', 'dayGrid', 'timeGrid', 'list',/*'bootstrap',*/  'googleCalendar', 'rrule', 'luxon', 'moment', 'momentTimezone' ],
          // themeSystem: 'bootstrap',
          /*
          header：ヘッダー：頭：カレンダーの上の部分。
          左、中央、右にそれぞれ要素が分かれる。
          ’’の中身はそれぞれカレンダー機能の正式名称。
          名称は変えないでください。
          機能一覧：全部試せばわかる。（けど書く）
          prev : 前へ
          next : 次へ
          today : 今日
          title : タイトル（だいたいその年）
          dayGridMonth : その月の予定を日単位でグリッド表示
          dayGridWeek : その週の予定を日単位でグリッド表示
          dayGridDay : その日の予定を日単位でグリッド表示
          timeGridWeek : その週の予定を時間単位でグリッド表示
          timeGridDay : その日の予定を時間単位でグリッド表示
          listYear : その年の予定をリスト表示
          listMonth : その月の予定をリスト表示
          listDay : その日の予定をリスト表示
          */
          header: {
            left: 'prev,next today',
            center: 'title',
            right: 'dayGridMonth,dayGridWeek,dayGridDay,timeGridWeek,timeGridDay,listYear,listMonth,listDay'
          },
          //リストビューに時間列を表示するかどうか
          displayEventTime: false, 
          //TODO 下準備で取得したAPIキーを''内に入力
          googleCalendarApiKey: 'A*************************************c',
          weekNumbers: true, //週ナンバーの有無（自由）
          eventLimit: true, // allow "more" link when too many events
          //TODO 下準備で用意した自分のGoogleカレンダーIDを''内に入力
          events:{
            googleCalendarId:'hogehoge@group.calendar.google.com'
          },
          //クリックしたら〜
          eventClick: function(arg) {
            // opens events in a popup window：ポップアップウィンドウでイベントを開きます
            window.open(arg.event.url, 'google-calendar-event', 'width=700,height=600');
            arg.jsEvent.preventDefault() // don't navigate in main tab：同じタブで開くな！
          },
          //カレンダー読み込み終わったら「loading...」の表示を隠す関数っぽい
          loading: function(bool) {
            document.getElementById('loading').style.display =
              bool ? 'block' : 'none';
          }
        });
        calendar.render();
      });
    </script>
    <style>
        body {
          margin: 40px 10px;
          padding: 0;
          font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
          font-size: 14px;
        }
        #loading {
          display: none;
          position: absolute;
          top: 10px;
          right: 10px;
        }
        #calendar {
          max-width: 900px;
          margin: 0 auto;
        }
      </style>
  </head>
  <body>
    <div id="loading">loading...</div>
    <div id="calendar"></div>
  </body>
</html>
```

できた？

- 次にソースコード中のAPIキー🔑とカレンダーIDの部分を書き換えてください。

- 最後に保存できたら適当なブラウザでindex.htmlを開いてください。できてると思います。

え？できない？そんな時はSlackの[DMへ](https://app.slack.com/client/TLMDF3ZQB/DLYS0AXGD)

おめでとう！
-----------

これでワクワクのCSSでのカレンダー魔改造を始められるね！

がんばってね👍

-------------------------------------------------------------------------------

ばっぐえんどは私とウキウキローカル開発でくぁｗせｄｒｆｔｇｙふじこｌｐ
======================================================================



END

p.s. フロント勢とやること一緒です。