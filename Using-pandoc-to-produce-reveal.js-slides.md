### Dependency Injection

 - 依存性注入
 - AngularJSでは普通に使われている
 - ALLINでもよく使ってます

---

### どこで使っている？

 - こういうの
```
    angular.module("app").controller("CockpitController", function(
        $scope,
        $state,
        $q,
        $translate,
        $log) 
```

---

### あれ？公式と書き方が違うけど・・・？

 - 公式では・・・
```
someModule.controller('MyController', ['$scope', 'greeter', function($scope, greeter) {
  // ...
}]);
```
 - 配列になってる・・・

---

### 配列で各メリットは？
 - 別名がつけられます
```
someModule.controller('MyController', ['$scope', 'greeter', function(a, b) {
  // ...
}]);
```
 - 配列アノテーションっていうらしい

---

### 別名をつけることのメリットは？
 - 短い変数名にできる
 - minify効果！

---

### ALLINでは？

 - 配列アノテーションは使ってない（ですかね？）
 - 可読性重視？

---

### ng-strict-di属性

 - AngularJS 1.3からの追加機能
 - ng-appの定義時に属性として付与する
 - strict = 厳格
 - 詳しくは[ここ](http://www.buildinsider.net/web/angularjstips/0004)
   （http://www.buildinsider.net/web/angularjstips/0004）
 - これを指定すると、<font color="red">**配列アノテーション必須**</font>となる。
 - <u>VersionUpしたら書き方変えましょう！</u>(minifyの予定があるなら)

---

### 




