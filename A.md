
# 0. 写在前面的话

很多同行在编写代码的时候往往只关注一些宏观上的主题：架构，设计模式，数据结构等等，却忽视了一些更细节上的点：比如变量如何命名与使用，控制流的设计，以及注释的写法等等。以上这些细节上的东西可以用**代码的可读性**来概括。

不同于宏观上的架构，设计模式等需要好几个类，好几个模块才能看出来：代码的可读性是**能够立刻从微观上的，一个变量的命名，函数的逻辑划分，注释的信息质量里面看出来的。**

宏观层面上的东西固然重要，但是代码的可读性也属于评价代码质量的一个无法让人忽视的指标：它影响了阅读代码的成本（毕竟代码是给人看的），甚至会影响代码出错的概率！

这里引用《编写可读代码的艺术》这本书里的一句话:

> 对于一个整体的软件系统而言，既需要宏观上的架构决策，设计与指导原则，也必须重视微观上的代码细节。在软件历史中，有许多影响深远的重大失败，其根源往往是编码细节出现了疏漏。

因此笔者认为代码的可读性可以作为考量一名程序员专业程度的指标。

或许已经有很多同行也正在努力提高自己代码的可读性。然而这里有一个很典型的错觉（笔者之前就有这种错觉）是：越少的代码越容易让人理解。

但是事实上，并不是代码越精简就越容易让人理解。相对于追求最小化代码行数，一个更好的提高可读性方法是**最小化人们理解代码所需要的时间。**

这就引出了这本中的一个核心定理：

> 可读性基本定理：代码的写法应当使别人理解它所需要的时间最小化。

正是这句话深深地吸引了我，于是决定利用这两周的业余时间读完并总结了这本书。

这本书讲的就是关于“如何提高代码的可读性”。
总结下来，这本书从浅入深，在三个层次告诉了我们如何让代码易于理解：

* 表层上的改进：在命名方法（变量名，方法名），变量声明，代码格式，注释等方面的改进。
* 控制流和逻辑的改进：在控制流，逻辑表达式上让代码变得更容易理解。
* 结构上的改进：善于抽取逻辑，借助自然语言的描述来改善代码。

# 1. 表层的改进

首先来讲最近简单的一层如何改进，涉及到以下几点：

* 如何命名
* 如何声明与使用变量
* 如何简化表达式
* 如何让代码具有美感
* 如何写注释

## 1.1 如何命名

> 关键思想：把尽可能多的信息装入名字中。

### 1.1.1 选择专业的词汇，避免泛泛的名字

一个比较常见的反例：`get` 。`get`这个词最好是用来做轻量级的取方法的开头，而如果用到其他的地方就会显得很不专业。

`getPage(url)` 通过这个方法名很难判断出这个方法是从缓存中获取页面数据还是从网页中获取。如果是从网页中获取，更专业的词应该是`fetchPage(url)`或者`downloadPage(url)`。

还有一个比较常见的反例：`returnValue`和`retval`。这两者都是“返回值”的意思，他们被滥用在各个有返回值的函数里面。其实这两个次除了携带他们本来的意思`返回值`以外并不具备任何其他的信息，是典型的泛泛的名字。

那么如何选择一个专业的词汇呢？答案是在非常贴近你自己的意图的基础上，选择一个富有表现力的词汇。

* 相对于`make`，选择`create`,`generate`,`build`等词汇会更有表现力，更加专业。
* 相对于`find`，选择`search`,`extract`,`recover`等词汇会更有表现力，更加专业。
* 相对于`retval`，选择一个能充分描述这个返回值的性质的名字。

但是，有些情况下，泛泛的名字也是有意义的，例如一个交换变量的情景：
```objective-c
if (right < left){
    tmp = right;
    right = left;
    left = tmp;
}
```
像上面这种`tmp`只是作为一个临时存储的情况下，tmp表达的意思就比较贴切了。因此，像tmp这个名字，只适用于短期存在而且特性为临时性的变量。

```objective-c
/**
 档案类别
 */
@interface YMMArticleGroupApiManager : BaseApiManager

/// 获取档案类别列表
+ (void)fetchArticleGroupListWithUid:(NSString *)uid
                             otherId:(NSString *)otherId
                             success:(void(^)(NSArray *array))success
                                fail:(void(^)(NSString *msg))fail;
                                
+ (void)fetchArticleGroupListWithUid:(NSString *)uid
                             otherId:(NSString *)otherId
                            isSeeImg:(BOOL)isSeeImg
                             success:(void(^)(NSArray *array))success
                                fail:(void(^)(NSString *msg))fail;

/// 编辑档案类别
+ (void)editArticleGroupWithGroupId:(NSString *)groupId
                          groupName:(NSString *)groupName
                           isPublic:(BOOL)isPublic
                            success:(void(^)(id response))success
                               fail:(void(^)(NSString *msg))fail;

/// 编辑档案类别公开状态
+ (void)editArticleGroupPublicStateWithGroupId:(NSString *)groupId
                                       success:(void(^)(id response))success
                                          fail:(void(^)(NSString *msg))fail;

/// 批量移动档案
+ (void)moveArticleWithImageIds:(NSString *)imageIds
                        groupId:(NSString *)groupId
                        success:(void(^)(NSString *msg))success
                           fail:(void(^)(NSString *msg))fail;

/// 新建档案类别并移动档案
+ (void)moveArticleWithImageIds:(NSString *)imageIds
                      groupName:(NSString *)groupName
                       isPublic:(NSString *)isPublic
                        success:(void(^)(NSString *msg))success
                           fail:(void(^)(NSString *msg))fail;

/// 批量删除档案
+ (void)deleteArticleWithImageIds:(NSString *)ImageIds
                          success:(void(^)(NSString *msg))success
                             fail:(void(^)(NSString *msg))fail;

/// 档案类别置顶
+ (void)topArticleGroupWithGroupId:(NSString *)groupId
                           success:(void(^)(NSString *msg))success
                              fail:(void(^)(NSString *msg))fail;
```

### 1.1.2 给名字附带更多信息

通用的约定，推荐使用长的、描述性的方法和变量名。

推荐:
```objective-c
UIButton *settingsButton;
UILabel *titleLabel
UITextField *passwordTextField
```

不推荐:
```objective-c
UIButton *setBut;
UILabel *title
UITextField *PwdTF
```

方法名与方法类型 (`-`/`+` 符号)之间应该以空格间隔。方法段之间也应该以空格间隔（以符合 Apple 风格）。参数前应该总是有一个描述性的关键词。尽可能少用 "and" 这个词。它不应该用来阐明有多个参数，比如下面的 `initWithWidth:height:` 这个例子：

推荐:
```objective-c
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

不推荐:
```objective-c
- (void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

使用字面值来创建不可变的 `NSString`, `NSDictionary`, `NSArray`, 和 `NSNumber` 对象。注意不要将 `nil` 传进 `NSArray` 和 `NSDictionary` 里，因为这样会导致崩溃。

```objective-c
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

不要这样:
```objective-c
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

如果要用到这些类的可变副本，我们推荐使用 `NSMutableArray`, `NSMutableString` 这样的类。

#### 1.1.2.1 为变量添加单位

有些变量是有单位的，在变量名的后面添加其单位可以让这个变量名携带更多信息：

* 一个表达时间间隔的变量，它的单位是秒：相对于`duraction`，`ducation_secs`携带了更多的信息
* 一个表达内存大小的变量，它的单位是`mb`：相对于`size`，`cache_mb`携带了更多的信息。

#### 1.1.2.2 为变量添加重要属性

有些变量是具有一些非常重要的属性，其重要程度是不允许使用者忽略的。例如：

* 一个UTF-8格式的`html`字节，相对于`html`，`html_utf8`更加清楚地描述了这个变量的格式。
* 一个纯文本，需要加密的密码字符串：相对于`password`，`plaintext_password`更清楚地描述了这个变量的特点。

#### 1.1.2.3 为变量选择适当的格式

对于命名，有些既定的格式需要注意：

* 使用大驼峰命名来表示类名：`HomeViewController`。
* 使用小驼峰命名来表示属性名：`userNameLabel`。
* 使用下划线连接词来表示变量名：`product_id`。
* 使用`kConstantName`来表示常量：`kCacheDuraction`。
* 使用MACRO_NAME来表示宏：`SCREEN_WIDTH`。

### 1.1.3 决定名字最适合的长度

#### 1.1.3.1 如果变量的作用域很小，可以取很短的名字

```swift
if(debug){
    map <string,int>m;
    LookUpNamesNumbers(&m);
    Print(m);
}
```
在这里，变量的类型和使用范围一眼可见，读者可以了解这段代码的所有信息，所以即使是取m这个非常简短的名字，也不影响读者来理解作者的意图。

相反的，如果`m`是一个全局变量，当你看到下面这段代码就会很头疼，因为你不知道它的类型并不明确:
```objective-c
LookUpNamesNumbers(&m);
Print(m);
```

#### 1.1.3.2 驼峰命名中的单元不能超过3个

我们知道驼峰命名可以很清晰地体现变量的含义，但是当驼峰命名中的单元超过了3个之后，就会很影响阅读体验:

`userFriendsInfoModel`

`memoryCacheCalculateTool`

#### 1.1.3.3 丢掉不必要的单元

有些单元在变量里面是可以去掉的，例如：

`convertToString`可以省略成`toString`。

#### 1.1.3.4 不能使用大家不熟悉的缩写

有些缩写是大家熟知的：

* `doc` 可以代替`document`
* `str` 可以代替`string`

但是如果你想用`BEManager`来代替`BackEndManager`就比较不合适了。因为不了解的人几乎是无法猜到这个名称的意义的。

所以类似这种情况不能偷懒，该是什么就是什么，否则会起到相反的效果。因为它看起来非常陌生，跟我们熟知的一些缩写规则相去甚远。

#### 1.1.3.5 名字不能引起歧义

有些名字会引起歧义，例如：

* filter：过滤这个词，可以是过滤出符合标准的，也可以是减少不符合标准的：是两种完全相反的结果，所以不推荐使用。
* clip：类似的，到底是在原来的基础上截掉某一段还是另外截出来某一段呢？同样也不推荐使用。
* 布尔值：read_password:是表达需要读取密码，还是已经读了密码呢？所以最好使用`need_password`或者`is_authenticated`来代替比较好。通常来说，给布尔值的变量加上`is`,`has,can`,`should`这样的词可以使布尔值表达的意思更加明确

有一个很简单的原则来判断这个变量名起的是否是好的：那就是：**团队的新成员是否能迅速理解这个变量名的含义。**如果是，那么这个命名就是成功的，否则就不要偷懒了，起个好名字，对谁都好。其实如果你养成习惯多花几秒钟想出个好名字，你会发现你的“命名能力”会很快提升。

## 1.2 如何声明与使用变量

在写程序的过程中我们会声明很多变量（成员变量，临时变量），而我们要知道变量的声明与使用策略是会对代码的可读性造成影响的：

* 变量越多，越难跟踪它们的动向。
* 变量的作用域越大，就需要跟踪它们的动向越久。
* 变量改变的越频繁，就越难跟踪它的当前值。

相对的，对于变量的声明与使用，我们可以从这四个角度来提高代码的可读性：

### 1.2.1 减少变量的个数

在一个函数里面可能会声明很多变量，但是有些变量的声明是毫无意义的，比如：

### 1.2.1.1 没有价值的临时变量

有些变量的声明完全是多此一举，它们的存在反而加大了阅读代码的成本：
```swift
let now = datetime.datatime.now()
root_message.last_view_time = now   
```
上面这个`now`变量的存在是毫无意义的，因为：

* 没有拆分任何复杂的表达式
* `datetime.datatime.now`已经很清楚地表达了意思
* 只使用了一次，因此而没有压缩任何冗余的代码

所以完全不用这个变量也是完全可以的：

`root_message.last_view_time = datetime.datatime.now()`

### 1.2.1.2 表示中间结果的变量

有的时候为了达成一个目标，把一件事情分成了两件事情来做，这两件事情中间需要一个变量来传递结果。但往往这件事情不需要分成两件事情来做，这个“中间结果”也就不需要了：

所以在写代码的时候，如果可以“速战速决”，就尽量使用最快，最简洁的方式来实现目的。

### 1.2.2 缩小变量的作用域

变量的作用域越广，就越难追踪它，值也越难控制，所以我们应该让你的**变量对尽量少的代码可见**。

比如类的成员变量就相当于一个“小型局部变量”。如果这个类比较庞大，我们就会很难追踪它，因为所有方法都可以“隐式”调用它。所以相反地，如果我们可以把它“降格”为局部变量，就会很容易追踪它的行踪：
```objective-c
// 成员变量，比较难追踪
class LargeCass{
  string str_;
  
  void Method1(){
     str_ = ...;
     Method2();
  }
  
  void Method2(){
     //using str_
  }
}
```
降格：
```objective-c
// 局部变量，容易追踪
class LargeCass{
  
  void Method1(){
     string str = ...;
     Method2(str);
  }
  
  void Method2(string str){
     //using str
  }
}
```
所以在设计类的时候如果这个数据（变量）可以通过方法参数来传递，就不要以成员变量来保存它。

### 1.2.3 缩短变量声明与使用其代码的距离

### 1.2.4 变量最好只写一次

操作一个变量的地方越多，就越难确定它的当前值。所以在很多语言里面有其各自的方式让一些变量不可变（是个常量），比如C++里的`const`和Java中的`final`。

### 1.2.5 如何简化表达式

#### 1.2.5.1 使用解释变量

有些变量会从一个比较长的算式得出，这个表达式可能很难让人看懂。这时候就需要用一个简短的“解释”变量来诠释算式的含义。使用书中的一个例子：

`if line.split(':')[0].strip() == "root"`
其实上面左侧的表达式其实得出的是用户名，我们可以用`username`来替换它：
```objective-c
username = line.split(':')[0].strip()
if username == "root"
```

#### 1.2.5.2 使用总结变量

除了以“变量”替换“算式”，还可以用“变量”来替换含有更多变量更复杂的内容，比如条件语句，这时候该变量可以被称为"总结变量"。使用书中的一个例子：
```objective-c
if(request.user.id == document.owner_id){
   //do something 
}
```
上面这条判断语句所判断的是：“该文档的所有者是不是该用户”。我们可以使用一个总结性的变量user_owns_document来替换它：
```objective-c
final boolean user_owns_document = (request.user.id == document.owner_id);
if (user_owns_document){
   //do something
}
```

#### 1.2.5.3 使用德摩根定理

德摩根定理:

1. `not(a or b or c)`等价于`(not a) and (not b) and (not c)`
2. `not(a and b and c)`等价于`(not a) or (not b) or (not c)`
当我们条件语句里面存在外部取反的情况，就可以使用德摩根定理来做个转换。使用书中的一个例子：

```objective-c
//使用德摩根定理转换以前
if(!(file_exists && !is_protected)){}

//使用德摩根定理转换以后
if(!file_exists || is_protected){}
```

## 1.3 如何让代码具有美感

在读过一些好的源码之后我有一个感受：好的源码往往都看上去都很漂亮，很有美感。这里说的漂亮和美感不是指代码的逻辑清晰有条理，而是指感官上的视觉感受让人感觉很舒服。这是从一种纯粹的审美的角度来评价代码的：富有美感的代码让人赏心悦目，也容易让人读懂。

### 1.3.1 空格

* 方法的大括号和其他的大括号(`if`/`else`/`switch`/`while` 等) 总是在同一行开始，在新起一行结束。

推荐:
```objective-c
if (user.isHappy) {
    //Do something
} 
else {
    //Do something else
}
```

不推荐:
```objective-c
if (user.isHappy)
{
  //Do something
} else {
  //Do something else
}
```

* 方法之间应该要有一个空行来帮助代码看起来清晰且有组织。 方法内的空格应该用来分离功能，但是通常不同的功能应该用新的方法来定义。
* 在实现文件中的声明应该新起一行。
* 应该总是让冒号对齐。有一些方法签名可能超过三个冒号，用冒号对齐可以让代码更具有可读性。即使有代码块存在，也应该用冒号对齐方法。

推荐:
```objective-c
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

不推荐:
```objective-c
[UIView animateWithDuration:1.0 animations:^{
    // something
} completion:^(BOOL finished) {
    // something
}];
```

如果自动对齐让可读性变得糟糕，那么应该在之前把 block 定义为变量，或者重新考虑你的代码签名设计。

#### 1.3.2 换行

换行比较常用在函数或方法的参数比较多的时候。

使用换行：
```objective-c
- (void)requestWithUrl:(NSString *)url 
                method:(NSString *)method 
                params:(NSDictionary *)params 
               success:(SuccessBlock)success 
               failure:(FailuireBlock)failure{
    
}
```

不使用换行：
```objective-c
- (void)requestWithUrl:(NSString*)url method:(NSString*)method params:(NSDictionary *)params success:(SuccessBlock)success failure:(FailuireBlock)failure{
    
}
```
通过比较可以看出，如果不使用换行，就很难一眼看清楚都是用了什么参数，而且代码整体看上去整洁干净了很多。
本指南关注代码显示效果以及在线浏览的可读性，所以换行是一个重要的主题。

```objective-c
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

一个像上面的长行的代码在第二行以一个间隔（2个空格）延续

```objective-c
self.productsRequest = [[SKProductsRequest alloc]
  initWithProductIdentifiers:productIdentifiers];
```

#### 1.3.3 列对齐

在声明一组变量的时候，由于每个变量名的长度不同，导致了在变量名左侧对齐的情况下，等号以及右侧的内容没有对齐：
```objective-c
NSString *name = userInfo[@"name"];
NSString *sex = userInfo[@"sex"];
NSString *address = userInfo[@"address"];
```
而如果使用了列对齐的方法，让等号以及右侧的部分对齐的方式会使代码看上去更加整洁：

````
NSString *name    = userInfo[@"name"];
NSString *sex     = userInfo[@"sex"];
NSString *address = userInfo[@"address"];
````

这二者的区别在条目数比较多以及变量名称长度相差较大的时候会更加明显。

### 1.3.4 选择一个有意义的顺序

当涉及到相同变量（属性）组合的存取都存在的时候，最好以一个有意义的顺序来排列它们：

* 让变量的顺序与对应的HTML表单中<input>字段的顺序相匹配
* 从最重要到最不重要排序
* 按照字母排序

举个例子：相同集合里的元素同时出现的时候最好保证每个元素出现顺序是一致的。除了便于阅读这个好处以外，也有助于能发现漏掉的部分，尤其当元素很多的时候：

```objective-c
//给model赋值
model.name    = dict["name"]；
model.sex     = dict["sex"]；
model.address = dict["address"]；

 ...
  
//拿到model来绘制UI
nameLabel.text    = model.name;
sexLabel.text     = model.sex;
addressLabel.text = model.address;


```

### 1.3.4 把代码分成"段落"

在写文章的时候，为了能让整个文章看起来结构清晰，我们通常会把大段文字分成一个个小的段落，让表达相同主旨的语言凑到一起，与其他主旨的内容分隔开来。

而且除了让读者明确哪些内容是表达同一主旨之外，把文章分为一个个段落的好处还有便于找到你的阅读”脚印“，便于段落之间的导航；也可以让你的阅读具有一定的节奏感。

其实这些道理同样适用于写代码：如果你可以把一个拥有好几个步骤的大段函数，以空行+注释的方法将每一个步骤区分开来，那么则会对读者理解该函数的功能有极大的帮助。这样一来，代码既能有一定的美感，也具备了可读性。其实可读性又何尝不是来自于规则，富有美感的代码呢？
```objective-c
BigFunction {
  
     //step1:*****
     ....
       
     //step2:*****
     ...
        
     //step3:*****
     ....
  
}
```

### 1.3.5 保持风格一致性

有些时候，你的某些代码风格可能与大众比较容易接受的风格不太一样。但是如果你在你自己所写的代码各处能够保持你这种独有的风格，也是可以对代码的可读性有积极的帮助的。

比如一个比较经典的代码风格问题：
```objective-c
if(condition) {

}
```
or:
```objective-c
if(condition)
{

}
```
对于上面的两种写法，每个人对条件判断右侧的大括号的位置会有不同的看法。**但是无论你坚持的是哪一个，请在你的代码里做到始终如一。因为如果有某几个特例的话，是非常影响代码的阅读体验的。**

我们要知道，一个逻辑清晰的代码也可以因为留白的不规则，格式不对齐，顺序混乱而让人很难读懂，这是十分让人痛心的事情。所以既然你的代码在命名上，逻辑上已经很优秀了，就不妨再费一点功夫把她打扮的漂漂亮亮的吧！

在以下的地方使用 [Egyptian风格 括号]（代码段括号的开始位于一行的末尾，而不是另外起一行的风格。)

* 控制语句 (if-else, for, switch)
* 类的实现
* 方法的实现


属性应该尽可能描述性地命名，避免缩写，并且是小写字母开头的驼峰命名。我们的工具可以很方便地帮我们自动补全所有东西。所以没理由少打几个字符了，并且最好尽可能在你源码里表达更多东西。

```objective-c
NSString *text;
```

不要这样 :
```objective-c
NSString* text;
NSString * text;
```

（注意：这个习惯和常量不同，这是主要从常用和可读性考虑。 C++ 的开发者偏好从变量名中分离类型，作为类型它应该是
`NSString*` （对于从堆中分配的对象，对于C++是能从栈上分配的）格式。）

当使用 setter getter 方法的时候尽量使用点符号。应该总是用点符号来访问以及设置属性。

```Objective-C
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;

```

不要这样:
```Objective-C
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

使用点符号会让表达更加清晰并且帮助区分属性访问和方法调用

推荐按照下面的格式来定义属性

```objective-c
@property (nonatomic, readwrite, copy) NSString *name;
```

属性的参数应该按照下面的顺序排列： 原子性，读写 和 内存管理。 这样做你的属性更容易修改正确，并且更好阅读。(译者注：习惯上修改某个属性的修饰符时，一般从属性名从右向左搜索需要修动的修饰符。最可能从最右边开始修改这些属性的修饰符，根据经验这些修饰符被修改的可能性从高到底应为：内存管理 > 读写权限 >原子操作)

为了完成一个共有的 getter 和一个私有的 setter，你应该声明公开的属性为 `readonly`  并且在类扩展中重新定义通用的属性为 `readwrite` 的。

```objective-c
//.h文件中
@interface MyClass : NSObject
@property (nonatomic, readonly, strong) NSObject *object;
@end
//.m文件中
@interface MyClass ()
@property (nonatomic, readwrite, strong) NSObject *object;
@end

@implementation MyClass
//Do Something cool
@end

```

私有属性应该定义在类的实现文件的类的扩展 (匿名的 category) 中。不允许在有名字的 category(如 `ZOCPrivate`）中定义私有属性，除非你扩展其他类。

```objective-c
@interface ZOCViewController ()
@property (nonatomic, strong) UIView *bannerView;
@end
```

### 1.3.6 利用代码块

一个 GCC 非常模糊的特性，以及 Clang 也有的特性是，代码块如果在闭合的圆括号内的话，会返回最后语句的值

```objective-c
NSURL *url = ({
    NSString *urlString = [NSString stringWithFormat:@"%@/%@", baseURLString, endpoint];
    [NSURL URLWithString:urlString];
});
```

### 1.3.7 Pragma Mark 

> code organization is a matter of hygiene  (代码组织是卫生问题)

我们十分赞成这句话。清晰地组织代码和规范地进行定义, 是你对自己以及其他阅读代码的人的尊重。

`#pragma mark -`  是一个在类内部组织代码并且帮助你分组方法实现的好办法。 我们建议使用  `#pragma mark -` 来分离:

- 不同功能组的方法
- protocols 的实现
- 对父类方法的重写

```objective-c

- (void)dealloc { /* ... */ }
- (instancetype)init { /* ... */ }

#pragma mark - View Lifecycle （View 的生命周期）

- (void)viewDidLoad { /* ... */ }
- (void)viewWillAppear:(BOOL)animated { /* ... */ }
- (void)didReceiveMemoryWarning { /* ... */ }

#pragma mark - Custom Accessors （自定义访问器）

- (void)setCustomProperty:(id)value { /* ... */ }
- (id)customProperty { /* ... */ }

#pragma mark - IBActions  

- (IBAction)submitData:(id)sender { /* ... */ }

#pragma mark - Public

- (void)publicMethod { /* ... */ }

#pragma mark - Private

- (void)zoc_privateMethod { /* ... */ }

#pragma mark - UITableViewDataSource

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath { /* ... */ }

#pragma mark - ZOCSuperclass

// ... 重载来自 ZOCSuperclass 的方法

#pragma mark - NSObject

- (NSString *)description { /* ... */ }

```

上面的标记能明显分离和组织代码。你还可以用  cmd+Click 来快速跳转到符号定义地方。
但是小心，即使 paragma mark 是一门手艺，但是它不是让你类里面方法数量增加的一个理由：类里面有太多方法说明类做了太多事情，需要考虑重构了。

# 2. 如何写注释

首先引用书中的一句话：

> 注释的目的是尽量帮助读者了解得和作者一样多。

在你写代码的时候，在脑海中可能会留下一些代码里面很难体现出来的部分：这些部分在别人读你的代码的时候可能很难体会到。而这些“不对称”的信息就是需要通过以注释的方式来告诉阅读代码的人。

想要写出好的注释，就需要首先知道：

* 什么不能作为注释
* 什么应该作为注释

## 2.1 什么不能作为注释

我们都知道注释占用了代码的空间，而且实际上对程序本身的运行毫无帮助，所以最好保证它是物有所值的。不幸的是，有一些注释是毫无价值的，它无情的占用了代码间的空间，影响了阅读代码的人的阅读效率，也浪费了写注释的人的时间。这样的注释有以下两种：

* 描述能立刻从代码自身就能立刻理解的代码意图的注释
* 给不好的命名添加的注释

### 2.1.1 描述能立刻从代码自身就能立刻理解的代码意图的注释
```objective-c
//add params1 and params2 and return sum of them
- (int)addParam1:(int)param1 param2:(int)param2
```
上面这个例子举的比较简单，但反映的问题很明显：这里面的注释是完全不需要的，它的存在反而增加了阅读代码的人的工作量。因为他从方法名就可以马上意会到这个函数的作用了。

### 2.1.2 给不好的命名添加的注释
```objective-c
- (void)
```

## 2.2 什么应该作为注释

本书中介绍的注释大概有以下几种：

* 写代码时的思考
* 对代码的评价
* 常量
* 全局观的概述

### 2.2.1 写代码时的思考

你的代码可能不是一蹴而就的，它的产生可能会需要一些思考的过程。然而很多时候代码本身却无法将这些思考表达出来，所以你就可能有必要通过注释的方式来呈现你的思考，让阅读代码的人知道这段代码是哪些思考的结晶，从而也让读者理解了这段代码为什么这么写。如果遇到了比你高明的高手，在他看到你的注释之后兴许会马上设计出一套更加合适的方案。

### 2.2.2 对代码的评价

有些时候你知道你现在写的代码是个临时的方案：它可能确实是解决当前问题的一个方法，但是：

* 你知道同时它也存在着某些缺陷，甚至是陷阱
* 你不知道有其他的方案可以替代了
* 你知道有哪个方案可以替代但是由于时间的关系或者自身的能力无法实现

也可能你知道你现在实现的这个方案几乎就是“完美的”，因为如果使用了其他的方案，可能会消耗更多的资源等等。

对于上面这些情况，你都有必要写上几个字作为注释来诚实的告诉阅读你的这段代码的人这段代码的情况，比如：
```objective-c
//该方案有一个很容易忽略的陷阱：****
//该方案是存在性能瓶颈，性能瓶颈在其中的**函数中
//该方案的性能可能并不是最好的，因为如果使用某某算法的话可能会好很多
```
## 2.3 常量

常量应该以驼峰法命名，并以相关类名作为前缀。
推荐使用常量来代替字符串字面值和数字，这样能够方便复用，而且可以快速修改而不需要查找和替换。常量应该用 `static` 声明为静态常量，而不要用 `#define`，除非它明确的作为一个宏来使用。

推荐:
```objective-c
static const NSTimeInterval ZOCSignInViewControllerFadeOutAnimationDuration = 0.4;
static NSString * const ZOCCacheControllerDidClearCacheNotification = @"ZOCCacheControllerDidClearCacheNotification";
static const CGFloat ZOCImageThumbnailHeight = 50.0f;
```

不推荐:
```objective-c
#define CompanyName @"Apple Inc."
#define magicNumber 42
```

常量应该在头文件中以这样的形式暴露给外部：

```objective-c
/// 注册成功
FOUNDATION_EXPORT NSString *const kRegisterSuccessNotification;
/// 登录成功
FOUNDATION_EXPORT NSString *const kLoginSuccessNotification;
/// 退出登录
FOUNDATION_EXPORT NSString *const kLogoutNotification;
/// 更新用户信息
FOUNDATION_EXPORT NSString *const kUpdateUserInfoNotification;
```
并在实现文件中为它赋值。
```objective-c
 // 注册成功
NSString *const kRegisterSuccessNotification = @"RegisterSuccessNotification";
// 登录成功
NSString *const kLoginSuccessNotification = @"LoginSuccessNotification";
// 退出登录
NSString *const kLogoutNotification = @"LogoutNotification";
// 更新用户信息
NSString *const kUpdateUserInfoNotification = @"UpdateUserInfoNotification";
```
### 2.3.1 通知
当你定义你自己的 `NSNotification` 的时候你应该把你的通知的名字定义为一个字符串常量，就像你暴露给其他类的其他字符串常量一样。你应该在公开的接口文件中将其声明为 `extern` 的， 并且在对应的实现文件里面定义。

```objective-c
// Foo.h
extern NSString * const ZOCFooDidBecomeBarNotification

// Foo.m
NSString * const ZOCFooDidBecomeBarNotification = @"ZOCFooDidBecomeBarNotification";
```

## 2.4 全局观的概述

对于一个刚加入团队的新人来说，除了团队文化，代码规范以外，可能最需要了解的是当前被分配到的项目的一些“全局观”的认识：比如组织架构，类与类之间如何交互，数据如何保存，如何流动，以及模块的入口点等等。

有时仅仅添加了几句话，可能就会让新人迅速地了解当前系统或者当前类的结构以及作用，而且这些也同样对开发过当前系统的人员迅速回忆出之前开发的细节有很大帮助。

这些注释可以在一个类的开头（介绍这个类的职责，以及在整个系统中的角色）也可以在一个模块入口处。书中举了一个关于这种注释的例子：
```objective-c
//这个文件包含了一些辅助函数，尾门的文件系统提供了更便利的接口
```
再举一个iOS开发里众所周知的网络框架`AFNetworking`的例子。在`AFHTTPSessionManager`的头文件里说明了这个类的职责：
```objective-c
//AFHTTPSessionManager` is a subclass of `AFURLSessionManager` with convenience methods for making HTTP requests. When a `baseURL` is provided, requests made with the `GET` / `POST` / et al. convenience methods can be made with relative paths
```
在知道了什么不应该是注释以及什么应该是注释以后，我们来看一下一个真正合格的注释应该是什么样子的：

> 注释应当有很高的信息/空间率

也就是说，注释应该用最简短的话来最明确地表达。要做到这一点需要做的努力是：

* 让注释保持紧凑：尽量用最简洁的话来表达，不应该有重复的内容
* 准确地描述函数的行为：要把函数的具体行为准确表达出来，不能停留在表明
* 用输入/输出的例子来说明特别的情况：有时相对于文字，可能用一个实际的参数和返回值就能立刻体现出函数的作用。而且有些特殊情况也可以通过这个方式来提醒阅读代码的人
* 声明代码的意图：也就是说明这段代码存在的意义，你为什么当时是这么写的原因

其实好的代码是自解释的，由于其命名的合理以及架构的清晰，几乎不需要注释来向阅读代码的人添加额外的信息，书中有一个公式可以很形象地表明一个好的代码本身的重要性：

> 好代码 > (坏代码 + 注释)


所有重要的方法，接口，分类以及协议定义应该有伴随的注释来解释它们的用途以及如何使用。更多的例子可以看 Google 代码风格指南中的 [File and Declaration Comments](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml#File_Comments)。

简而言之：有长的和短的两种字符串文档。

短文档适用于单行的文件，包括注释斜杠。它适合简短的函数，特别是（但不仅仅是）非 public 的 API：

```
// Return a user-readable form of a Frobnozz, html-escaped.
```

如果描述超过一行，应改用长字符串文档：

 * 以`/**`开始
 * 换行写一句总结的话，以`?或者!或者.`结尾。
 * 空一行
 * 在与第一行对齐的位置开始写剩下的注释
 * 最后用`*/`结束。

```
/**
 This comment serves to demonstrate the format of a docstring.

 Note that the summary line is always at most one line long, and
 after the opening block comment, and each line of text is preceded
 by a single space.
*/
```

一个函数必须有一个字符串文档，除非它符合下面的所有条件：

* 非公开
* 很短
* 显而易见

字符串文档应该描述函数的调用符号和语义，而不是它如何实现。

注释应该用来解释特定的代码做了什么。所有的注释必须被持续维护或者干脆就删掉。

一个类的文档应该只在 .h 文件里用 Doxygen/AppleDoc 的语法书写。 方法和属性都应该提供文档。

```objective-c
/**
 *  Designated initializer.
 *
 *  @param  store  The store for CRUD operations.
 *  @param  searchService The search service used to query the store.
 *
 *  @return A ZOCCRUDOperationsStore object.
 */
- (instancetype)initWithOperationsStore:(id<ZOCGenericStoreProtocol>)store
                          searchService:(id<ZOCGenericSearchServiceProtocol>)searchService;
```

# 3. 控制流和逻辑的改进

## 3.1 条件语句

条件语句体应该总是被大括号包围。尽管有时候你可以不使用大括号（比如，条件语句体只有一行内容），但是这样做会带来问题隐患。比如，增加一行代码时，你可能会误以为它是 if 语句体里面的。此外，更危险的是，如果把 if 后面的那行代码注释掉，之后的一行代码会成为 if 语句里的代码。

推荐:
```objective-c
if (!error) {
    return success;
}
```
不推荐:
```objective-c
// eg.1 
if (!error)
    return success;

// eg.2
if (!error) return success;
```

此外，在其他条件语句里面也应该按照这种风格统一，这样更便于检查。

### 3.1.1 尤达表达式

不要使用尤达表达式。尤达表达式是指，拿一个常量去和变量比较而不是拿变量去和常量比较。它就像是在表达 “蓝色是不是天空的颜色” 或者 “高个是不是这个男人的属性” 而不是  “天空是不是蓝的” 或者 “这个男人是不是高个子的”

推荐:
```objective-c
if ([myValue isEqual:@42]) { ...
```

不推荐:
```objective-c
if ([@42 isEqual:myValue]) { ...
```

推荐:
```objective-c
if (someObject) { ...
if (![someObject boolValue]) { ...
if (!someObject) { ...
```

不推荐:
```objective-c
if (someObject == YES) { ... // Wrong
if (myRawValue == YES) { ... // Never do this.
if ([someObject boolValue] == NO) { ...
```
同时这样也能提高一致性，以及提升可读性。

### 3.1.2 黄金大道

在使用条件语句编程时，代码的左边距应该是一条“黄金”或者“快乐”的大道。 也就是说，不要嵌套 `if` 语句。使用多个 return 可以避免增加循环的复杂度，并提高代码的可读性。因为方法的重要部分没有嵌套在分支里面，并且你可以很清楚地找到相关的代码。

推荐:
```objective-c
- (void)someMethod {
  if (![someOther boolValue]) {
      return;
  }
  //Do something important
}
```

不推荐:
```objective-c
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

### 3.1.3 三元运算符

三元运算符 ? 应该只用在它能让代码更加清楚的地方。 一个条件语句的所有的变量应该是已经被求值了的。类似 if 语句，计算多个条件子句通常会让语句更加难以理解。或者可以把它们重构到实例变量里面。

推荐:
```objective-c
result = a > b ? x : y;
```

不推荐:
```objective-c
result = a > b ? x = c > d ? c : d : y;
```

当三元运算符的第二个参数（if 分支）返回和条件语句中已经检查的对象一样的对象的时候，下面的表达方式更灵巧：

推荐:
```objective-c
result = object ? : [self createObject];
```

不推荐:
```objective-c
result = object ? object : [self createObject];
```

## 3.1.4 Case语句

除非编译器强制要求，括号在 case 语句里面是不必要的。但是当一个 case 包含了多行语句的时候，需要加上括号。

```objective-c
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces
        break;
       }
    case 3:
        // ...
        break;
    default:
        // ...
        break;
}
```

有时候可以使用 fall-through 在不同的 case 里面执行同一段代码。一个 fall-through  是指移除 case 语句的 “break” 然后让下面的 case 继续执行。

```objective-c
switch (condition) {
    case 1:
    case 2:
        // code executed for values 1 and 2
        break;
    default:
        // ...
        break;
}
```

当在 switch 语句里面使用一个可枚举的变量的时候，`default` 是不必要的。比如：

```objective-c
switch (menuType) {
    case ZOCEnumNone:
        // ...
        break;
    case ZOCEnumValue1:
        // ...
        break;
    case ZOCEnumValue2:
        // ...
        break;
}
```

此外，为了避免使用默认的 case，如果新的值加入到 enum，程序员会马上收到一个 warning 通知

`Enumeration value 'ZOCEnumValue3' not handled in switch.（枚举类型 'ZOCEnumValue3' 没有被 switch 处理）`

### 3.1.5 枚举类型

当使用 `enum` 的时候，建议使用新的固定的基础类型定义，因为它有更强大的类型检查和代码补全。 SDK 现在有一个 宏来鼓励和促进使用固定类型定义 - `NS_ENUM()`

```objective-c
typedef NS_ENUM(NSUInteger, ZOCMachineState) {
    ZOCMachineStateNone,
    ZOCMachineStateIdle,
    ZOCMachineStateRunning,
    ZOCMachineStatePaused
};
```

控制流在编码中占据着很重要的位置，它往往代表着一些核心逻辑和算法。因此，如果我们可以让控制流变得看上去更加“自然”，那么就会对阅读代码的人理解这些逻辑甚至是整个系统提供很大的帮助。

那么都有哪相关实践呢？

* 使用符合人类自然语言的表达习惯
* if/else语句块的顺序
* 使用return提前返回

## 3.2 使用符合人类自然语言的表达习惯

写代码也是一个表达的过程，虽然表现形式不同，但是如果我们能够采用符合人类自然语言习惯的表达习惯来写代码，对阅读代码的人理解我们的代码是很有帮助的。

这里有两个比较典型的情景：

* 条件语句中参数的顺序
* 条件语句中的正负逻辑

### 3.2.1 条件语句中参数的顺序：

首先比较一下下面两段代码，哪一个更容易读懂？
```objective-c
//code 1
if(length > 10)

//code 2
if(10 < length)
```
大家习惯上应该会觉得code1容易读懂。

再来看下面一个例子：
```objective-c
//code 3
if(received_number < standard_number) 

//code 4
if( standard_number< received_number)
```
仔细看会发现，和上面那一组情况类似，大多数人还是会觉得code3更容易读懂。

那么 code1 和 code3 有什么共性呢？

它们的共性就是：**左侧都是被询问的内容（通常是一个变量）；右侧都是用来做比较的内容（通常是一个常量）**

这应该是符合自然语言的一个顺序。比如我们一般会说“今天的气温大于20摄氏度”，而不习惯说“20摄氏度小于今天的气温”。

### 3.2.2 条件语句中的正负逻辑：

在判断一些正负逻辑的时候，建议使用`if(result)`而不是`if(!result)`。

因为大脑比较容易处理正逻辑，比如我们可能比较习惯说“某某某是个男人”，而不习惯说“某某某不是个女人”。如果我们使用了负逻辑，大脑还要对它进行取反，相当于多做了一次处理。

## 3.3 if/else语句块的顺序

在写if/else语句的时候，可能会有很多不同的互斥情况（好多个`else if`）。那么这些互斥的情况可以遵循哪些顺序呢？

* **先处理掉简单的情况，后处理复杂的情况：**这样有助于阅读代码的人循序渐进地地理解你的逻辑，而不是一开始就吃掉一个胖子，耗费不少精力。
* **先处理特殊或者可疑的情况，后处理正常的情况：**这样有助于阅读代码的人会马上看到当前逻辑的边界条件以及需要注意的地方。

## 3.4 使用return提前返回

在一个函数或是方法里，可能有一些情况是比较特殊或者极端的，对结果的产生影响很大（甚至是终止继续进行）。如果存在这些情况，我们应该把他们写在前面，用`return`来提前返回（或者返回需要返回的返回值）。

这样做的好处是可以减少if/else语句的嵌套，也可以明确体现出：“哪些情况是引起异常的”。

再举一个JSONModel里的例子，在initWithDictionary:error方法里面就有很多return操作，它们都体现出了“在什么情况下是不能成功将字典转化为model对象”的；而且在方法的最后返回了对象，说明如果到了这一步，则在转化的过程中通过了层层考验：

```objective-c
- (id)initWithDictionary:(NSDictionary*)dict error:(NSError**)err {
    // check for nil input
    if (!dict) {
        if (err) *err = [JSONModelError errorInputIsNil];
        return nil;
    }

    // invalid input, just create empty instance
    if (![dict isKindOfClass:[NSDictionary class]]) {
        if (err) *err = [JSONModelError errorInvalidDataWithMessage:@"Attempt to initialize JSONModel object using initWithDictionary:error: but the dictionary parameter was not an 'NSDictionary'."];
        return nil;
    }

    // create a class instance
    self = [self init];
    if (!self) {
        // super init didn't succeed
        if (err) *err = [JSONModelError errorModelIsInvalid];
        return nil;
    }

    // check incoming data structure
    if (![self __doesDictionary:dict matchModelWithKeyMapper:self.__keyMapper error:err]) {
        return nil;
    }

    // import the data from a dictionary
    if (![self __importDictionary:dict withKeyMapper:self.__keyMapper validation:YES error:err]) {
        return nil;
    }

    // run any custom model validation
    if (![self validate:err]) {
        return nil;
    }

    // model is valid! yay!
    return self;
}
```

# 4. 代码组织的改进

关于代码组织的改进，作者介绍了以下三种方法:

* 抽取出与程序主要目的“不相关的子逻辑”
* 重新组织代码使它一次只做一件事情
* 借助自然语言描述来将想法变成代码

## 4.1 抽取出与程序主要目的“不相关的子逻辑”

一个函数里面往往包含了其主逻辑与子逻辑，我们应该积极地发现并抽取出与主逻辑不相关的子逻辑。具体思考的步骤是：

1. 首先确认这段代码的高层次目标是什么（主要目标）？
2. 对于每一行代码，都要反思一下：“它是直接为了目标而工作么？”
3. 如果答案是肯定的并且这些代码占据着一定数量的行数，我们就应该将他们抽取到独立的函数中。

比如某个函数的目标是**为了寻找距离某个商家最近的地铁口，**那么这其中一定会重复出现一些计算两组经纬度之间距离的子逻辑。但是这些子逻辑的具体实现是不应该出现在这个主函数里面的，因为这些细节与这个主函数的目标来讲应该是无关的。

即是说，像这种类似于工具方法的函数其实是脱离于某个具体的需求的：它可以用在其他的主函数中，也可以放在其他的项目里面。比如**找到离运动场场最近的几个公交站**这个需求等等。

而像这种“抽取子逻辑或工具方法”的做法有什么好处呢？

* 提高了代码的可读性：将函数的调用与原来复杂的实现进行替换，让阅读代码的人很快能了解到该子逻辑的目的，让他们把注意力放在更高层的主逻辑上，而不会被子逻辑的实现（往往是复杂无味的）所影响。
* 便于修改和调试：因为一个项目中可能会多次调用该子逻辑（计算距离，计算汇率，保留小数点），当业务需求发生改变的时候只需要改变这一处就可以了，而且调试起来也非常容易。
* 便于测试：同理，也是因为可以被多次调用，在进行测试的时候就比较有针对性。

从函数扩大到项目，其实在一个项目里面，有很多东西不当前这个项目所专有的，它们是可以用在其他项目中的一些“通用代码”。这些通用代码可以对当前的项目一无所知，可以被用在其他任何项目中去。

我们可以养成这个习惯，“把一般代码与项目专有代码分开”，并不断扩大我们的通用代码库来解决更多的一般性问题。

## 4.2 重新组织代码使它一次只做一件事情

一个比较大的函数或者功能可能由很多任务代码组合而来，在这个时候我们有必要将他们分为更小的函数来调用它们。

这样做的好处是：我们可以清晰地看到这个功能是人如何一步一步完成的，而且拆分出来的小的函数或许也可以用在其他的地方。

所以如果你遇到了比较难读懂的代码，可以尝试将它所做的所有任务列出来。可能马上你就会发现这其中有些任务可以转化成单独的函数或者类。而其他的部分可以简单的成为函数中的一个逻辑段落。

## 4.3 借助自然语言描述来将想法变成代码

在设计一个解决方案之前，如果你能够用自然语言把问题说清楚会对整个设计非常有帮助。因为如果直接从大脑中的想法转化为代码，可能会露掉一些东西。

但是如果你可以将整个问题和想法滴水不漏地说出来，就可能会发现一些之前没有想到的问题。这样可以不断完善你的思路和设计。



# 5. 最后想说的

这本书从变量的命名到代码的组织来讲解了一些让代码的可读性提高的一些实践方法。

其实笔者认为代码的可读性也可以算作是一种沟通能力的一种体现。因为写代码的过程也可以被看做是写代码的人与阅读代码的人的一种沟通，只不过这个沟通是单向的：代码的可读性高，可以说明写代码的人思路清晰，而且TA可以明确，高效地把自己的思考和工作内容以代码的形式表述出来。
所以笔者相信能写出可读性很高的代码的人，TA对于自己的思考和想法的描述能力一定不会很差。

如果你真的打算好好做编程这件事情，建议你从最小的事情上做起：好好为你的变量起个名字。不要再以“我英语不好”或者“没时间想名字”作为托辞；把态度端正起来，平时多动脑，多查字典，多看源码，自然就会了。

如果你连起个好的变量名都懒得查个字典，那你怎么证明你在遇到更难的问题的时候能够以科学的态度解决它？
如果你连编程里这种最小的事情都不好好做，那你又怎么证明你对编程是有追求的呢？


#  6. 其它

##  6.1 类名

类名应该以至少两个大写字母作为前缀。尽管这个规范看起来有些古怪，但是这样做可以减少 Objective-C 没有命名空间所带来的问题。

另一个好的类的命名规范：当你创建一个子类的时候，你应该把说明性的部分放在前缀和父类名的在中间。

举个例子：如果你有一个 `ZOCNetworkClient` 类，子类的名字会是`ZOCTwitterNetworkClient` (注意 "Twitter" 在 "ZOC" 和 "NetworkClient" 之间); 按照这个约定， 一个`UIViewController` 的子类会是 `ZOCTimelineViewController`.

## 6.2 Initializer 和 dealloc

推荐的代码组织方式是将 `dealloc` 方法放在实现文件的最前面，`init` 应该跟在 `dealloc` 方法后面。

如果有多个初始化方法， 指定初始化方法 (designated initializer) 应该放在最前面，间接初始化方法 (secondary initializer) 跟在后面，这样更有逻辑性。如今有了 ARC，dealloc 方法几乎不需要实现，不过把 init 和 dealloc 放在一起可以从视觉上强调它们是一对的。通常，在 init 方法中做的事情需要在 dealloc 方法中撤销。

`init` 方法应该是这样的结构：

```objective-c
- (instancetype)init {
    self = [super init]; // call the designated initializer
    if (self) {
        // Custom initialization
    }
    return self;
}
```

### 6.3 Designated 和 Secondary 初始化方法

Objective-C 有指定初始化方法(designated initializer)和间接(secondary initializer)初始化方法的观念。
designated 初始化方法是提供所有的参数，secondary 初始化方法是一个或多个，并且提供一个或者更多的默认参数来调用 designated 初始化的初始化方法。

```objective-c
@implementation ZOCEvent

- (instancetype)initWithTitle:(NSString *)title
                         date:(NSDate *)date
                     location:(CLLocation *)location {
    self = [super init];
    if (self) {
        _title    = title;
        _date     = date;
        _location = location;
    }
    return self;
}

- (instancetype)initWithTitle:(NSString *)title
                         date:(NSDate *)date {
    return [self initWithTitle:title date:date location:nil];
}

- (instancetype)initWithTitle:(NSString *)title {
    return [self initWithTitle:title date:[NSDate date] location:nil];
}

@end
```

`initWithTitle:date:location:` 就是 designated 初始化方法，另外的两个是 secondary 初始化方法。因为它们仅仅是调用类实现的 designated 初始化方法

#### 6.3.1 Designated Initializer

一个类应该有且只有一个 designated 初始化方法，其他的初始化方法应该调用这个 designated 的初始化方法（虽然这个情况有一个例外）

#### 6.3.2 Secondary Initializer

secondary initializer 是一种提供默认值、行为到 designated initializer的方法。也就是说，在这样的方法里面你不应该有初始化实例变量的操作，并且你应该一直假设这个方法不会得到调用。我们保证的是唯一被调用的方法是 designated initializer。
这意味着你的 secondary initializer 总是应该调用 Designated initializer  或者你自定义(上面的第三种情况：自定义Designated initializer)的 `self`的 designated initializer。有时候，因为错误，可能打成了  `super`，这样会导致不符合上面提及的初始化顺序（在这个特别的例子里面，是跳过当前类的初始化）

### 6.4 instancetype

我们经常忽略 Cocoa 充满了约定，并且这些约定可以帮助编译器变得更加聪明。无论编译器是否遭遇 `alloc` 或者 `init` 方法，他会知道，即使返回类型都是 `id` ，这些方法总是返回接受到的类类型的实例。因此，它允许编译器进行类型检查。（比如，检查方法返回的类型是否合法）。Clang的这个好处来自于 [related result type](http://clang.llvm.org/docs/LanguageExtensions.html#related-result-types)， 意味着：

> messages sent to one of alloc and init methods will have the same static type as the instance of the receiver class （发送到 alloc 或者 init 方法的消息会有同样的静态类型检查是否为接受类的实例。）

一个相关的返回类型可以明确地规定用 `instancetype` 关键字作为返回类型，并且它可以在一些工厂方法或者构造器方法的场景下很有用。它可以提示编译器正确地检查类型，并且更加重要的是，这同时适用于它的子类。

```objective-c
@interface ZOCPerson
+ (instancetype)personWithName:(NSString *)name;
@end
```

虽然如此，根据 clang 的定义，`id` 可以被编译器提升到 `instancetype` 。在 `alloc` 或者 `init` 中，我们强烈建议对所有返回类的实例的类方法和实例方法使用 `instancetype` 类型。

在你的 API 中要构成习惯以及保持始终如一的，此外，通过对你代码的小调整你可以提高可读性：在简单的浏览的时候你可以区分哪些方法是返回你类的实例的。你以后会感谢这些注意过的小细节的。

###  6.5 初始化模式

#### 6.5.1 类簇 （class cluster)

类簇在Apple的文档中这样描述：

> an architecture that groups a number of private, concrete subclasses under a public, abstract superclass. （一个在共有的抽象超类下设置一组私有子类的架构）

如果这个描述听起来很熟悉，说明你的直觉是对的。 Class cluster 是 Apple 对[抽象工厂](https://en.wikipedia.org/wiki/Abstract_factory_pattern)设计模式的称呼。

class cluster 的想法很简单: 使用信息进行(类的)初始化处理期间，会使用一个抽象类（通常作为初始化方法的参数或者判定环境的可用性参数）来完成特定的逻辑或者实例化一个具体的子类。而这个"Public Facing（面向公众的）"类，必须非常清楚他的私有子类，以便在面对具体任务的时候有能力返回一个恰当的私有子类实例。对调用者来说只需知道对象的各种API的作用即可。这个模式隐藏了他背后复杂的初始化逻辑，调用者也不需要关心背后的实现。

Class clusters 在 Apple 的Framework 中广泛使用：一些明显的例子比如  `NSNumber` 可以返回不同类型给你的子类，取决于 数字类型如何提供  (Integer, Float, etc...) 或者 `NSArray` 返回不同的最优存储策略的子类。

这个模式的精妙的地方在于，调用者可以完全不管子类，事实上，这可以用在设计一个库，可以用来交换实际的返回的类，而不用去管相关的细节，因为它们都遵从抽象超类的方法。

我们的经验是使用类簇可以帮助移除很多条件语句。

一个经典的例子是如果你有为 iPad 和 iPhone 写的一样的 UIViewController 子类，但是在不同的设备上有不同的行为。比较基础的实现是用条件语句检查设备，然后执行不同的逻辑。虽然刚开始可能不错，但是随着代码的增长，运行逻辑也会趋于复杂。一个更好的实现的设计是创建一个抽象而且宽泛的 view controller 来包含所有的共享逻辑，并且对于不同设备有两个特别的子例。

通用的 view controller  会检查当前设备并且返回适当的子类。

```objective-c
@implementation ZOCKintsugiPhotoViewController

- (id)initWithPhotos:(NSArray *)photos {
    if ([self isMemberOfClass:ZOCKintsugiPhotoViewController.class]) {
        self = nil;

        if ([UIDevice isPad]) {
            self = [[ZOCKintsugiPhotoViewController_iPad alloc] initWithPhotos:photos];
        } else {
            self = [[ZOCKintsugiPhotoViewController_iPhone alloc] initWithPhotos:photos];
        }
        return self;
    }
    return [super initWithNibName:nil bundle:nil];
}

@end
```

####  6.5.3 单例

如果可能，请尽量避免使用单例而是依赖注入。然而，如果一定要用，请使用一个线程安全的模式来创建共享的实例。对于 GCD，用 `dispatch_once()` 函数就可以咯。

```objective-c
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;
   static dispatch_once_t onceToken = 0;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });
   return sharedInstance;
}
```

 `dispatch_once()`  的优点是，它更快，而且语法上更干净，因为dispatch_once()的意思就是 “把一些东西执行一次”，就像我们做的一样。

单例通常公开一个`sharedInstance`的类方法就已经足够了，没有任何的可写属性需要被暴露出来。尝试着把单例作为一个对象的容器，在代码或者应用层面上共享，是一个糟糕和丑陋的设计。

> NOTE：单例模式应该运用于类及类的接口趋向于作为单例来使用的情况 


### 6.7 可变对象

任何可以用一个可变的对象设置的（(比如 `NSString`,`NSArray`,`NSURLRequest`)）属性的内存管理类型必须是 `copy` 的。

这是为了确保防止在不明确的情况下修改被封装好的对象的值(译者注：比如执行 array(定义为 copy 的 NSArray 实例) = mutableArray，copy 属性会让 array 的 setter 方法为 array = [mutableArray copy], [mutableArray copy] 返回的是不可变的 NSArray 实例，就保证了正确性。用其他属性修饰符修饰，容易在直接赋值的时候，array 指向的是 NSMuatbleArray 的实例，在之后可以随意改变它的值，就容易出错)。

你应该同时避免暴露在公开的接口中可变的对象，因为这允许你的类的使用者改变类自己的内部表示并且破坏类的封装。你可以提供可以只读的属性来返回你对象的不可变的副本。

```objective-c
/* .h */
@property (nonatomic, readonly) NSArray *elements

/* .m */
- (NSArray *)elements {
  return [self.mutableElements copy];
}
```

### 6.8 懒加载（Lazy Loading）

当实例化一个对象需要耗费很多资源，或者配置一次就要调用很多配置相关的方法而你又不想弄乱这些方法时，我们需要重写 getter 方法以延迟实例化，而不是在 init 方法里给对象分配内存。通常这种操作使用下面这样的模板：

```objective-c
- (NSDateFormatter *)dateFormatter {
  if (!_dateFormatter) {
    _dateFormatter = [[NSDateFormatter alloc] init];
        NSLocale *enUSPOSIXLocale = [[NSLocale alloc] initWithLocaleIdentifier:@"en_US_POSIX"];
        [_dateFormatter setLocale:enUSPOSIXLocale];
        [_dateFormatter setDateFormat:@"yyyy-MM-dd'T'HH:mm:ss.SSS"];//毫秒是SSS，而非SSSSS
  }
  return _dateFormatter;
}

```

即使这样做在某些情况下很不错，但是在实际这样做之前应当深思熟虑。事实上，这样的做法是可以避免的。

##  方法

### 参数断言

你的方法可能要求一些参数来满足特定的条件（比如不能为nil），在这种情况下最好使用 `NSParameterAssert()` 来断言条件是否成立或是抛出一个异常。

###  私有方法

永远不要在你的私有方法前加上 `_` 前缀。这个前缀是 Apple 保留的。不要冒重载苹果的私有方法的险。

# Categories

虽然我们知道这样写很丑, 但是我们应该要在我们的 category 方法前加上自己的小写前缀以及下划线，比如`- (id)zoc_myCategoryMethod`。

这是非常必要的。因为如果在扩展的 category 或者其他 category 里面已经使用了同样的方法名，会导致不可预计的后果。实际上，**实际被调用的是最后被加载的那个 category 中方法的实现**(译者注：如果导入的多个 category 中有一些同名的方法导入到类里时，最终调用哪个是由编译时的加载顺序来决定的，最后一个加载进来的方法会覆盖之前的方法)。

一个好的实践是在 category 名中使用前缀。

```objective-c
@interface NSDate (ZOCTimeExtensions)
- (NSString *)zoc_timeAgoShort;
@end
```

分类可以用来在头文件中定义一组功能相似的方法。这是在 Apple的 Framework 也很常见的一个实践（下面例子的取自`NSDate` 头文件）。我们也强烈建议在自己的代码中这样使用。

我们的经验是，创建一组分类对以后的重构十分有帮助。一个类的接口增加的时候，可能意味着你的类做了太多事情，违背了类的单一功能原则。

之前创造的方法分组可以用来更好地进行不同功能的表示，并且把类打破在更多自我包含的组成部分里。

```objective-c
@interface NSDate : NSObject <NSCopying, NSSecureCoding>
@property (readonly) NSTimeInterval timeIntervalSinceReferenceDate;
@end

@interface NSDate (NSDateCreation)
+ (instancetype)date;
+ (instancetype)dateWithTimeIntervalSinceNow:(NSTimeInterval)secs;
+ (instancetype)dateWithTimeIntervalSinceReferenceDate:(NSTimeInterval)ti;
+ (instancetype)dateWithTimeIntervalSince1970:(NSTimeInterval)secs;
+ (instancetype)dateWithTimeInterval:(NSTimeInterval)secsToBeAdded sinceDate:(NSDate *)date;
// ...
@end
```
