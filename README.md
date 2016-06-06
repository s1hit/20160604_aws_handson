# 20160604_aws_handson

## P.25
```
http://weather.livedoor.com/forecast/webservice/json/v1?city=320010
```

## P.29
```
$input.json('$')
```


## P.35
```
application/json;charset=utf-8
```


## P.56
```
#set($inputRoot = $input.path('$'))
{
  "title" : "$inputRoot.title",
  "text" : "$inputRoot.description.text"
}
```


## P.73
```
'use strict';

let AWS = require('aws-sdk');
let qs = require('querystring');

// エコーBot(slackからのメッセージをそのまま返す)
exports.handler = (event, context, callback) => {
  let token = event.slackToken;
  let body = event.body;
  let params = qs.parse(body);
  
  if (token === '' || token !== params.token) {
    // Tokenが一致しない場合は処理しない(不正アクセス防止)
    callback(null, "Invalid request token");
    
  } else if (params.user_name === 'slackbot') {
    // Botからのメッセージには反応しない(無限ループ防止)
    callback(null, 'This event skipped because user is bot');
    
  } else {
    // 受信したテキストをそのまま送信
    callback(null, {"text": params.text});
  }
};
```



## P.91
```
application/x-www-form-urlencoded
```


## P.92
```
{
  "body": $input.json("$"),
  "slackToken": "$stageVariables.slackToken"
}
```



## P.125
```
'use strict';

let doc = require('dynamodb-doc');
let dynamo = new doc.DynamoDB();

//  全国の地点定義データをDynamoDBに投入する
exports.handler = (event, context, callback) => {
  var items = [];
  for (let record of records) {
    items.push(
      // DynamoDBへの項目追加処理
      dynamo.putItem({
        TableName: 'Cities',
        Item: {
          cityName: record[0],
          cityId: record[1]
        }
      }).promise() // 非同期処理なのでpromiseでラップする
    );
  }
  
  // 非同期処理を実行
  Promise.all(items).then(() => {
    // すべて成功した場合
    callback(null, 'ok');
  }).catch((error) => {
    // 失敗がひとつでもあった場合
    callback(error);
  });
}

// 全国の地点定義データ
let records = [
  ['稚内', '011000'],
  ['旭川', '012010'],
  ['留萌', '012020'],
  ['網走', '013010'],
  ['北見', '013020'],
  ['紋別', '013030'],
  ['根室', '014010'],
  ['釧路', '014020'],
  ['帯広', '014030'],
  ['室蘭', '015010'],
  ['浦河', '015020'],
  ['札幌', '016010'],
  ['岩見沢', '016020'],
  ['倶知安', '016030'],
  ['函館', '017010'],
  ['江差', '017020'],
  ['青森', '020010'],
  ['むつ', '020020'],
  ['八戸', '020030'],
  ['盛岡', '030010'],
  ['宮古', '030020'],
  ['大船渡', '030030'],
  ['仙台', '040010'],
  ['白石', '040020'],
  ['秋田', '050010'],
  ['横手', '050020'],
  ['山形', '060010'],
  ['米沢', '060020'],
  ['酒田', '060030'],
  ['新庄', '060040'],
  ['福島', '070010'],
  ['小名浜', '070020'],
  ['若松', '070030'],
  ['水戸', '080010'],
  ['土浦', '080020'],
  ['宇都宮', '090010'],
  ['大田原', '090020'],
  ['前橋', '100010'],
  ['みなかみ', '100020'],
  ['さいたま', '110010'],
  ['熊谷', '110020'],
  ['秩父', '110030'],
  ['千葉', '120010'],
  ['銚子', '120020'],
  ['館山', '120030'],
  ['東京', '130010'],
  ['大島', '130020'],
  ['八丈島', '130030'],
  ['父島', '130040'],
  ['横浜', '140010'],
  ['小田原', '140020'],
  ['新潟', '150010'],
  ['長岡', '150020'],
  ['高田', '150030'],
  ['相川', '150040'],
  ['富山', '160010'],
  ['伏木', '160020'],
  ['金沢', '170010'],
  ['輪島', '170020'],
  ['福井', '180010'],
  ['敦賀', '180020'],
  ['甲府', '190010'],
  ['河口湖', '190020'],
  ['長野', '200010'],
  ['松本', '200020'],
  ['飯田', '200030'],
  ['岐阜', '210010'],
  ['高山', '210020'],
  ['静岡', '220010'],
  ['網代', '220020'],
  ['三島', '220030'],
  ['浜松', '220040'],
  ['名古屋', '230010'],
  ['豊橋', '230020'],
  ['津', '240010'],
  ['尾鷲', '240020'],
  ['大津', '250010'],
  ['彦根', '250020'],
  ['京都', '260010'],
  ['舞鶴', '260020'],
  ['大阪', '270000'],
  ['神戸', '280010'],
  ['豊岡', '280020'],
  ['奈良', '290010'],
  ['風屋', '290020'],
  ['和歌山', '300010'],
  ['潮岬', '300020'],
  ['鳥取', '310010'],
  ['米子', '310020'],
  ['松江', '320010'],
  ['浜田', '320020'],
  ['西郷', '320030'],
  ['岡山', '330010'],
  ['津山', '330020'],
  ['広島', '340010'],
  ['庄原', '340020'],
  ['下関', '350010'],
  ['山口', '350020'],
  ['柳井', '350030'],
  ['萩', '350040'],
  ['徳島', '360010'],
  ['日和佐', '360020'],
  ['高松', '370000'],
  ['松山', '380010'],
  ['新居浜', '380020'],
  ['宇和島', '380030'],
  ['高知', '390010'],
  ['室戸岬', '390020'],
  ['清水', '390030'],
  ['福岡', '400010'],
  ['八幡', '400020'],
  ['飯塚', '400030'],
  ['久留米', '400040'],
  ['佐賀', '410010'],
  ['伊万里', '410020'],
  ['長崎', '420010'],
  ['佐世保', '420020'],
  ['厳原', '420030'],
  ['福江', '420040'],
  ['熊本', '430010'],
  ['阿蘇乙姫', '430020'],
  ['牛深', '430030'],
  ['人吉', '430040'],
  ['大分', '440010'],
  ['中津', '440020'],
  ['日田', '440030'],
  ['佐伯', '440040'],
  ['宮崎', '450010'],
  ['延岡', '450020'],
  ['都城', '450030'],
  ['高千穂', '450040'],
  ['鹿児島', '460010'],
  ['鹿屋', '460020'],
  ['種子島', '460030'],
  ['名瀬', '460040'],
  ['那覇', '471010'],
  ['名護', '471020'],
  ['久米島', '471030'],
  ['南大東', '472000'],
  ['宮古島', '473000'],
  ['石垣島', '474010'],
  ['与那国島', '474020']
];
```


## P.140
```
'use strict';

let AWS = require('aws-sdk');
let qs = require('querystring');
let doc = require('dynamodb-doc');
let dynamo = new doc.DynamoDB();

// エコーBot(slackからのメッセージをそのまま返す)
exports.handler = (event, context, callback) => {
  let token = event.slackToken;
  let body = event.body;
  let requestParams = qs.parse(body);
    
  if (token === '' || token !== requestParams.token) {
    // Tokenが一致しない場合は処理しない(不正アクセス防止)
    callback(null, "Invalid request token");
    
  } else if (requestParams.user_name === 'slackbot') {
    // Botからのメッセージには反応しない(無限ループ防止)
    callback(null, 'This event skipped because user is bot');
    
  } else {
    // Botの名前
    let botName = '@bot';
    
    // 処理対象メッセージチェックの正規表現(slackからBot宛のメンションにのみ反応する)
    let match = requestParams.text.match(new RegExp(botName + '\\s([\\s\\S]+)'));
    
    if (match) {
      // Bot に対するメッセージを処理する
      let bot = new Bot();
      bot.execute(match[1], callback);
      
    } else {
      // Bot に対するメッセージではない
      callback(null, 'This event skipped:' + requestParams.text);
    }
  }
};

class Bot {
  
  // Bot起動
  execute(command, callback) {
    // コマンド(/echoや/cityなど)によりアクションを振り分ける
    if (command.match(/\/echo\s([\s\S]*)/)) {
      // echoアクション
      this.echo(RegExp.$1, callback);

    } else if (command.match(/\/city\s(.+)/)) {
      // cityアクション
      this.city(RegExp.$1, callback);

    } else {
      // マッチしない場合
      callback(null, {text: '/echo または /city で話しかけてください。'});
    }
  }
  
  // echoアクション
  echo(text, callback) {
    // 受信したテキストをそのまま送信
    callback(null, {text: text});
  }
  
  // cityアクション
  city(cityName, callback) {
    // cityNameからcityIdを取得する
    dynamo.getItem({
      TableName: 'Cities',
      Key: {cityName: cityName}
    }).promise().then((data) => {
      // 検索が成功した場合
      
      if (data.Item) {
        // cityName が見つかった
        callback(null, {text: cityName + 'のIDは' + data.Item.cityId + 'です。'});
        
      } else {
        // 検索結果なし
        callback(null, {text: cityName + 'は見つかりませんでした。'});
      }
    }).catch((error) => {
      // 検索が失敗した場合
      callback(null, {text: 'エラーが発生しました。'});
    });
  }
}
```
