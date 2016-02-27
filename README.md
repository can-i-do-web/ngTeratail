# ハンズオン

## ディレクトリを作成する

```
ngDemo
    ∟ index.html
```

## index.html の準備

+ HTML の記述
+ Angular の読み込み(CDN)

  `https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js`


参考までに開いておきましょう

[API Document](https://docs.angularjs.org/api)

## ローカルサーバを起動する(できない場合は file://... でOK)

Python
```
ngDemo 直下で...

python -m SimpleHTTPServer
```

Ruby
```
ngDemo 直下で...

ruby -run -e httpd -- -p 8000
              or
ruby -run -e httpd ./ -p 8000 
```

ローカルサーバの動作確認する
```
localhost:8000 にアクセス
```

## AngularJS の読み込み確認

+ {{ 1+1 }} が動けばOK！
+ {{ いろいろやってみよう }}

## データバインディングしてみる

## Filter つかってみる

+ currency
+ currency(show '¥')
+ currency(calc tax)

```
{{ todo | currency: '¥': 0 }}
```

## Form Validation 

+ required min-length max-length
+ form と name 追加する
+ エラーメッセージを追加する
+ ボタンを作成する
+ エラーメッセージを表示する
+ エラー時TextBoxの枠を赤くする
```
<style>
  input.ng-invalid{
    border: solid 1px red;
  }
</style>
```


## Controller つくる

+ appCtrl つくる
```
angular.module('myApp', [])

  .controller('appCtrl', [function(){
    this.clickButton = function(){
      alert('click!!');
    };
  }]);
  
```

+ ng-controller で紐付ける

    `<body ng-controller="appCtrl as app">`

    `<input type="text" name="todo" ng-model="app.todo.name" required>`

    `<button ng-click="app.clickButton()" ng-disabled="todoForm.todoName.$invalid">click!!</button>`


+ ボタンを押して click!! とアラートが表示されればOK!

## 一覧表示させてみよう

+ list に入力した文字を保存しておく
```
angular.module('myApp', [])
  
  .controller('appCtrl', [function(){
    this.list = [];
    this.clickButton = function(){
      this.todo.date = Date.now();
      this.list.push(this.todo)
      this.todo = {};
    }
  }]);

```

+ 入力した文字を表示させる

```
<ul>
  <li ng-repeat="item in app.list">
    {{item.name}}
  </li>
</ul>
```

```
<ul>
  <li ng-repeat="item in app.list">
    <span>{{item.name}} - {{item.date | date: 'yyyy/MM/dd HH:mm:ss'}}</span>
  </li>
</ul>
```

## 絞り込み機能をつくる

+ input[ng-model="app.serch"] 追加する
+ orderBy で並び替え

```
... | orderBy: '-date'
```

## クリックしたら 打ち消し線で消そう

```
  .done{
    text-decoration: line-through;
  }
```


```
<li ng-repeat="item in app.list">
  <span
    ng-click="app.clickList(item)
    ng-class="{done: item.done} >...</span>
</li>
```

```
this.list = [];
this.clickButton = function(){
  this.todo.date = Date.now();
  this.list.push(this.todo)
  this.todo = {};
}
this.clickList = function(item){
  item.done = !item.done;
}
```

## 削除してみよう

+ 削除ボタンを追加する

```
<li>...
<button ng-click="app.clickDeleteButton(item)" ng-show="item.done">X</button>
...</li>
```

```
angular.module('myApp', [])
  
  .controller('appCtrl', [function(){
    this.list = [];
    this.clickButton = function(){
      this.todo.date = Date.now();
      this.list.push(this.todo)
      this.todo = {};
    }
    this.clickDeleteButton = function(item){
      var idx = this.list.indexOf(item);
      if(idx >=0){
        this.list.splice(idx, 1);
      }
    }
    
  }]);
```

## 最後は Component 化！

```
angular.module('myApp', [])
  .component('todo', {
    controllerAs: 'app',
    controller: function(){
      this.list = [];
      this.clickButton = function(){
        this.todo.date = Date.now();
        this.list.push(this.todo)
        this.todo = {};
      }
      this.clickList = function(item){
        item.done = !item.done;
      }
      this.clickDeleteButton = function(item){
        var idx = this.list.indexOf(item);
        if(idx >=0){
          this.list.splice(idx, 1);
        }
      }
    },
    templateUrl: 'todo.html'
  })
})();
```

※ file:// の人は template: `...`
