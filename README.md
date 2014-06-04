# CVTE Objective-C 代码规范

这份规范大部分参照了[@raywenderlich](http://raywenderlich.com)的代码规范。

## 目录

* [语言](#语言)
* [代码结构](#代码结构)
* [缩进](#缩进)
* [注释](#注释)
* [命名](#命名)
* [方法](#方法)
* [变量](#变量)
* [属性特性](#属性特性)
* [点表示法](#点表示法)
* [Literals](#literals)
* [枚举类型](#枚举类型)
* [Case语句](#Case语句)
* [布尔变量](#布尔变量)
* [条件语句](#条件语句)
* [三元运算](#三元运算)
* [构造函数和内存释放](#构造函数和内存释放)
* [类构造函数](#类构造函数)
* [CGRect函数](#CGRect函数)
* [黄金路径](#黄金路径)
* [错误捕获](#错误捕获)
* [单例](#单例)
* [换行](#换行)
* [Xcode项目](#Xcode项目)


## 语言

使用英式英语

**Preferred:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**
```objc
UIColor *myColour = [UIColor whiteColor];
```


## 代码结构

使用 `#pragram mark - ` 区分不同功能的代码块

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## 缩进

* 使用Xcode默认的缩进风格

* 方法大括号和其它大括号（比如if/else/switch/while等等）应在语句的同一行开始，而在新的一行关闭。  

**Preferred:**
```objc
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```

**Not Preferred:**
```objc
if (user.isHappy)
{
    //Do something
}
else {
    //Do something else
}
```

* 为保证视觉上的整洁和代码组织，在方法之间应提供且仅提供一行空白。方法中的空白应用于区分功能，但空白行最好用于区分两个不同方法。
* 为避免错误，条件语句体必须使用大括号。

**Preferred:**
```objc
if (!error) {
  return success;
} 
```

**Not Preferred:**
```objc
if (user.isHappy)
	return success;
```

* block的代码块居左。尽量避免和对应函数签名对齐。对齐的可阅读性太差。

**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## 注释

代码应该是自解释的。只有在必要的时候在对应代码做相应注释，表明为什么使用这段代码。所有的代码注释必须是最新的，否则就必须删掉。应尽量使用行注释，而避免使用块注释。之所以这样是因为代码自身需要是自文档化的，因此只需要零散添加一些行注释。当然，对于用于生成文档的注释，该原则并不适用。

## 命名

尽可能的遵守苹果的命名规范, 特别是有关[内存管理](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)的命名 ([NARC](http://stackoverflow.com/a/2865194/340508)).

尽可能使用全称，可描述的语言来命名属性和方法

**Preferred:**

```objc
UIButton *settingsButton;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

对于类和常量名称，应尽量使用三大写字母前缀（比如LYI），但对Core Data的实体名称可不适用该法则。
常量名称应首先在头部加’k’表示常量,然后加上相关类的名称作为前缀，其他的单词采用驼峰命名法.

**Preferred:**

```objc
static NSTimeInterval const kNavigationFadeAnimationDuration = 0.3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

属性名称应使用camel-case（驼峰式）命名方法，且第一个单词的首字母应为小写。如果Xcode版本支持对变量的自动合成，则不必深究。否则与该属性对应的实例变量名称的第一个单词的首字母应为小写，且在前面加上下划线。

**Preferred:**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
```

### 下划线

当使用属性变量时，应通过self.来获取和更改实例变量。这就意味着所有的属性将是独特的，因为它们的名称前会加上self.。本地变量名称中不应包含下划线。

## 方法

(+/-)符号和方法签名之间空一格 (遵循苹果的声明风格).  在参数之前包含一格关键词来描述参数的含义。

尽量避免在方法签名中包含单词`and`。推荐如下用法：`initWithWidth:height:`

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## 变量

变量的命名应尽可能具有自解释性。除了在for()循环语句中，应避免使用单个字母变量名称。

除非是常量，星号应紧贴变量名称表示指向变量的指针，除了const常量的声明。比如：
正确用法： `NSString *text` 
不当用法： `NSString* text` 或 `NSString * text`

[私有属性](#private-properties)   
私有属性应在类实现文件的类扩展（匿名分类）中进行声明。应避免使用命名分类.

应该要避免直接访问属性（通过getter/setter以及点操作）。除了在初始化的方法中(`init`, `initWithCoder:`, etc…), `dealloc` 方法 以及自定义的 setters 与 getters 方法. 更多关于getter/setter的hi使用请看 [这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**

```objc
@interface RWTTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```


## 属性特性

参照从IB中拉线出来生成的属性方式。内存管理修饰符在前，原子操作描述放在后边。因为内存管理相对会重要一些，方便阅读时首先能看到此修饰。

**Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```



## 点表示法

应“仅”用于获取和改变属性。括号表示法用于所有其它实例。

**Preferred:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

在创建`NSString`,`NSDictionary`,`NSArray`和`NSNumber`等对象的immutable实例时，应使用字面量。需要注意的是，不应将nil传递给`NSArray`和`NSDictionary`字面量，否则会引起程序崩溃。

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## 枚举类型

在使用enum的时候，推荐适用最新的fixed underlying type(WWDC 2012 session 405- Modern Objective-C)规范，因为它具备更强的类型检查和代码完成功能。

**For Example:**

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};
```

或者可以指定枚举的值：

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```

不推荐使用以下方式声明常量：

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


## Case语句

除非编译器报错，或者包含多行的代码，否则不需要在case语句使用大括号。

```objc
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

优势多个case可能会使用一段代码，那么就使用fall-through的方式，即去掉case语句的break

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}

```

当使用枚举类型时，不需要添加default。比如：

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```

## 布尔变量

因为nil将被解析为NO，因此没有必要在条件语句中进行比较。永远不要将任何东西和YES进行直接比较，因为YES被定义为1，而一个BOOL变量可以有8个字节。


**Preferred:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**Not Preferred:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

如果一个BOOL属性使用形容词来表达，setter忽略’is’前缀，getter添加"is"前缀


```objc
@property (assign, getter=isEditable) BOOL editable;
```
命名规则来自 [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## 条件语句

为避免错误，条件语句体必须使用大括号。

**Preferred:**
```objc
if (!error) {
  return success;
}
```

**Not Preferred:**
```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### 三元运算

可以让代码显得更清晰易懂时方可使用三元运算。更多情况下应使用条件语句。使用类似if的条件语句对多种条件进行判断通常要更容易理解，或使用实例变量

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## 构造函数和内存释放

构造返回值使用instancetype，代码格式使用apple提供的模板

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

更多关于instancetype请参见 [Class Constructor Methods](#class-constructor-methods) 

dealloc方法应放在方法实现文件的顶部

## 类构造函数

使用类构造函数时，使用instancetype作为返回值

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(RWTAirplaneType)type;
@end
```

更多关于instancetype请参见 [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect函数

当需要获取一个CGRect矩形的x,y,width,height属性时，应使用[`CGGeometry`函数](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) ，而非直接访问结构体成员。

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## 黄金路径

尽可能不要嵌套if else，需要返回时直接返回。

**Preferred:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**Not Preferred:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## 错误捕获

当可以根据函数返回值判断错误时，不要通过error变量来判断。

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

一些apple的API会在成功时在error参数（如果不为NULL时）里写垃圾信息，所以使用error可能会导致判断错误和崩溃。

## 单例

在创建单例对象的共享实例时，应使用线程安全模式。
```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## 换行

在代码过长时使用分行来保证代码的可读性。

例如:
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

```objc
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
```

## Xcode项目

为避免文件混乱，实际的物理文件路径应和Xcode项目内的路径保持一直。在Xcode中所创建的任何group都应有文件系统中相对应的文件夹。不应仅根据文件类型来进行分组，还需要考虑到其作用。
在Xcode的target的Build Setting中，中尽量开启”Treat Warnings as Errors“，同时尽量开启其他的警告[additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)。如果需要忽略一些特别的警告，请使用[Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# 其他的Objective-C代码规范

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
