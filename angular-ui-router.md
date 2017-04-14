###第二章【路由嵌套】

#####路由嵌套（视图和状态嵌套）
状态是可以相互嵌套的，通常使用以下几种方式：
- 在状态名称中使用点标记来嵌套，如：`.state('contacts.list',{})`.
  这种方法是最常用的，ui-router会自动根据你使用的点标记语法来推断出状态的层级关系。如：
  ```javascript
      //这里的 contacts 状态下面有一个子状态
      $stateProvider
      .state('contacts', {})
      .state('contacts.list', {});
  ```
- 在状态的`parent`属性中指定父级状态，`parent`属性的值可以为`string`或者`object`，如：
  ```javascript
      //parent使用string时，应该指定为状态名称
      //这里 list 状态中并没有使用点标记来标明层级关系，不过在 list 状态中有一个parent属性指定了其父级状态的名称
      $stateProvider
      .state('contacts', {})
      .state('list', {
        parent: 'contacts'
      });
    ```
	```javascript
      //parent使用object时，应该指定为父级状态的对象
      //这里 list 状态中也没有使用点标记来标明层级关系，不过在 list 状态中有一个parent属性指定了其父级状态对象

      //这里定义一个状态名为contacts
      var contacts = {
            name: 'contacts',
            templateUrl: 'contacts.html'
      }
      //这里定义一个状态名为contactsList
      var contactsList = {
          name: 'list',
          parent: contacts,  //指定它的父状态为contacts
          templateUrl: 'contacts.list.html'
      }

      $stateProvider
        .state(contacts)
        .state(contactsList)
      ```
#####状态定义顺序
通常来说，定义的顺序对代码没什么影响，你可以任意的定义。比如：父状态定义在子状态之后，跨模块定义等。因为最终他们终将会排成队列初始化的，像树一样，从上倒下初始化。
不过如果你定义了一个子状态，那么它们的父状态却是一定要定义的。不然代码会报错^_^。还有，在同一个父状态定义多个子状态时，名称应该是唯一的。



#####状态嵌套与视图
一个子状态激活后，与之相关的父状态也会隐式激活。每个状态的视图将会嵌入到父状态模版的`ui-view`标签中。
子状态将会继承到父状态定义的解决器(resolve)和自定义数据(data)


#####抽象状态
抽象状态是不能激活的（但它可以有子状态）也就是说它只能激活下面的子状态。一般在下面情况可能会用到：
* 为子状态分类添加`url`
* 多个`ui-view`下子状态插入节点控制
* 为子状态提供解决器和自定义数据
* 为子状态提供统一的`onEnter`或`onExit`事件
请记住抽象状态至少需要一个`ui-view`提供给子状态使用，所以抽象状态都应该有`template`属性。

一些例子：
```javascript
$stateProvider
	//这里为子状态提供统一的层级URL
    .state('contacts', {
        abstract: true,
        url: '/contacts',

        // 这里至少应该为子状态的模版提供一个填充位
        template: '<ui-view/>'
    })
    .state('contacts.list', {
        // url 应是 '/contacts/list'
        url: '/list'
        //...more
    })
    .state('contacts.detail', {
        // url 应是 '/contacts/detail'
        url: '/detail',
        //...more
    })

```
```javascript
$stateProvider
	//组织多个子视图
    .state('contacts', {
        abstract: true,
        templateUrl: 'contacts.html'
    })
    .state('contacts.list', {
        //填充到父模版中
        templateUrl: 'contacts.list.html'
    })
    .state('contacts.detail', {
        //填充到父模版中
        templateUrl: 'contacts.detail.html'
    })

```
当然你也可以组合起来使用：
```javascript
$stateProvider
	//提供统一的URL/父级controller/和自定义数据contacts
	$stateProvider
    .state('contacts', {
        abstract: true,
        url: '/contacts',
        templateUrl: 'contacts.html',
        controller: function($scope){
            $scope.contacts = [{ id:0, name: "Alice" }, { id:1, name: "Bob" }];
        }
    })
    .state('contacts.list', {
        url: '/list',
        templateUrl: 'contacts.list.html'
    })
    .state('contacts.detail', {
        url: '/:id',
        templateUrl: 'contacts.detail.html',
        controller: function($scope, $stateParams){
          $scope.person = $scope.contacts[$stateParams.id];
        }
    })
```

###第三章【多视图的使用】
到现在你至少应该明白`ui-view`标签就是为子状态预留模版插入位置的标记，那么在多视图或复杂的视图中，多个`ui-view`和视图又如何对应呢，主要有以下几种访视。
实现如下图的视图：
![示例图片](images/MultipleNamedViewsExample.png)
这里可以将页面分为三部分：过滤条件/列表/图表。

这样我们就可以拆成三个模版，分别定义如下：
第一种，这里使用的是名称匹配,也是最简单的一种
```html
<!-- index.html -->
<body>
  <div ui-view="filters"></div>
  <div ui-view="tabledata"></div>
  <div ui-view="graph"></div>
</body>
```
```javascript
$stateProvider
  .state('report', {
  //这里的三个属性分别与上面ui-view对应,请注意：当设置views后，状态的templateUrl/template/templateProvider属性将会被忽略
    views: {
      'filters': { ... templates and/or controllers ... },
      'tabledata': {},
      'graph': {},
    }
  })
```
第二种，视图的相对名称和绝对名称
视图和状态后台进行绑定的模型是`viewname@statename`而statename就是定义状态时的名称,所以它可以不写。这样上面的例子也可以这样写：
 ```javascript
 .state('report',{
    views: {
      'filters@': { },
      'tabledata@': { },
      'graph@': { }
    }
  })
```
相对名称
  ```javascript
 $stateProvider
  .state('contacts', {
    //它的父级应该是index.html
    templateUrl: 'contacts.html'
  })
  .state('contacts.detail', {
    views: {
        ////////////////////////////////////
        // 相对名称                         //
        // 相对于改状态的父状态 ui-view's     //
        ////////////////////////////////////

        //将模版嵌入到'contacts'状态中的ui-view='detail'标签中
        // <div ui-view='detail'/> within contacts.html
        "detail" : { },

        //将模版嵌入到contacts状态中的无名的ui-view标签中
        // <div ui-view/> within contacts.html
        "" : { },

        /////////////////////////////////////////////////////////////////
        // 绝对名称请使用 '@'符号                                         //
        // 这里可以使用任意的视图和状态名称，请记住viewname@statename这个模型 //
        ///////////////////////////////////////////////////////////////

        //将模版嵌入contacts.detail状态里的ui-view='info'标签的位置
        // <div ui-view='info'/> within contacts.detail.html
        "info@contacts.detail" : { }

        //将模版嵌入到contacts状态里的ui-view='detail'标签的位置
        // <div ui-view='detail'/> within contacts.html
        "detail@contacts" : { }

        //省略了视图名称，也就是将模版嵌入到ui-view=''的标签位置
        // <div ui-view/> within contacts.html
        "@contacts" : { }

        //省略了状态,那么就应该是嵌入到index.html里的ui-view='status'标签位置
        // <div ui-view='status'/> within index.html
        "status@" : { }

        //省略了视图名和状态名。应该嵌入到index.html里的ui-view=''标签位置
        // <div ui-view/> within index.html
        "@" : { }
  });
 ```

###第四章【URL路由】
通过URL可以访问到你定义的多个状态，比如你这样设置路由：
```javascript
$stateProvider
  $stateProvider
    .state('contacts', {
        url: "/contacts",
        templateUrl: 'contacts.html'
    })
  })
```
这样你就可以`index.html/contacts`来激活`contacts`状态，contacts.html将会替换到父级的`ui-view`标签中而且相应的地址也会变为`index.html/contacts`

#####URL传参
通常你可以在激活一个状态时给其传递参数，比如：
```javascript
$stateProvider
    .state('contacts.detail', {
        url: "/contacts/:contactId",
        templateUrl: 'contacts.detail.html',
        controller: function ($stateParams) {
            // If we got here from a url of /contacts/42
            expect($stateParams).toBe({contactId: "42"});
        }
    })
```
这样我们定义了一个可以传参的状态（请注意url属性的配置），其中上面的url属性也可以这样写：
```javascript
url: "/contacts/{contactId}"
```
这里的意思是，在URL路径中可以在contacts后面跟一个任意的字符串，我们把这个字符串变量名命名为contactId.其中还有更多用法：
* `/hello/` -这个只是匹配/hello/这种url路径
* `/hello/:id` -这个可以在hello后面跟任意字符串做参数
* `/hello/{id}` -这只是上面的另一种写法
* `/hello/{id:int} -这个说明了变量id的数据类型为int
* `/hello/{id:[0-9]{3}}` -这个使用了一个正则表达式来确定参数id的可传入类型
注意这里的变量名和编程中的变量名取名规则一致，也就是说只能使用数字、字母、下划线，且第一个不能为数字

很好，现在我们已经定义了一个可以传入参数的状态。那么我们应该怎么传入参数呢？
* 在URL路径中直接携带参数，如`/hello/123`这里的id的值就是123了
* 在链接中使用
 ```javascript
 	<a ui-sref="contacts.detail({contactId: id})">View Contact</a>
 ```

前面的参数传递类似于http的get请求，传递参数时会在url中显示。对于这种情况ui-router还提供了另一种方式可以隐藏传参：
定义方式如下：
```javascript
 	.state('contacts', {
        url: "/contacts",
        params: {
            param1: null
        },
        templateUrl: 'contacts.html'
    })
```
那么它又是怎么传入：
* 通过链接传入
 ```html
 	<a ui-sref="contacts({param1: value1})">View Contacts</a>
 ```
* 通过函数传入
 ```javascript
 	$state.go('contacts', {param1: value1})
 ```

传参和参数定义已经介绍完了，那么我们怎么在我们的controller中接收这些参数呢，这就要使用到`$stateParams`服务了。其实上面的参数都会在stateparams服务中将变量名注册为key并注入相应的值，其用法如下：
```javascript
 	$stateProvider.state('contacts.detail', {
       url: '/contacts/:contactId',
       controller: function($stateParams){
          $stateParams.contactId  //*** 可以访问未定义的属性，不过值为undefined ***//
       }
    }).state('contacts.detail.subitem', {
       url: '/item/:itemId',
       controller: function($stateParams){//注入服务
          $stateParams.contactId //*** 这里可以直接使用 ***//
          $stateParams.itemId //*** Exists! ***//
       }
    })
```

#####路由事件处理（$urlRouterProvider）
$urlRouterProvider这个服务可以为URL跳转做一些事件操作，它的基本原型如下：
```javascript
 	app.config(function($urlRouterProvider){
    //跳往空链接则跳往主页
    $urlRouterProvider.when('', '/index');

    //跳往/aspx/i时转向主页
    $urlRouterProvider.when('/aspx/i', '/index');

    //如果你把参数作为函数的话，angular会为这个函数启用注入器的
    $urlRouterProvider.when('/aspx/i', function($match, $stateParams) {

    });

    //如果访问URL的没有定义，则跳往主页
    $urlRouterProvider.otherwise('/index');
})
```

###组件介绍
* $state/$stateProvider: 状态定义管理器
* ui-sref 指令: 功效和HTML中的A标签差不多
* ui-view 指令： 标记子状态模版替换位置
* $urlRouter/$urlRouterProvider： 管理url和状态之间的变化，它的等级最低
* $urlMatcherFactory： 编译URL路径
* $templateFactory： http加载状态的视图模版