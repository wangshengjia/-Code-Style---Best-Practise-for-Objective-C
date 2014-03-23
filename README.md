Code Style & Best Practise for Objective-C
===========================================

##Language
* Il faut toujours utiliser l’anglais par défaut.

**Preferred:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**
```objc
UIColor *monColour = [UIColor whiteColor];
```

##Code Naming

[Apple Official Naming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146-SW1)

**Preferred:**
Clear, expressive, non-ambiguous names. Since you do far more reading of code than writing, invest the time to type an extra eight characters (and then leverage autocomplete).

**Not Preferred:**
* Hungarian notation
  e.g. data type as part of name `strUserName`  
  Exception: `const` or `enum` values should follow Apple's class-name-then-least-to-most specific pattern. e.g. `UIDeviceOrientationLandscapeRight` or `VPNavigationBarHeight_iPad`).

* Abbreviating. Lazy naming.
  e.g `index` What index?
  For an array of house objects use `houseIndex`

##Method Signatures

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
- (instancetype)initWith:(int)width and:(int)height;
```

**Preferred:**
```objc
answerViewController
```

**Not Preferred:**
```objc
answer_view_controller
```

* For methods that represent an action an object takes, start the name with the action.

**Preferred:**
```objc
- (IBAction)showDetailViewController:(id)sender
```

**Not Preferred:**
```objc
- (void)detailButtonTapped:(id)sender
```

If the method returns an attribute of the receiver, name the method after the attribute.  The use of "get" is unnecessary, unless one or more values are returned indirection.

**Preferred:**
```objc
- (NSInteger)age
```

**Not Preferred:**
```objc
- (NSInteger)calcAge
- (NSInteger)getAge
```


##Variables

* Use @property without @synthesized is always preferred in most cases in stead of using directly instance variable. 

Direct instance variable access should be avoided except one exception to this: inside initializers, the backing instance variable (i.e. _variableName) should be used directly to avoid any potential side effects of the getters/setters.

* Local variables should not contain underscores.

* Use always NSInteger/NSUInteger instead of int. Use CGFloat instead float.

##Expressions

* Dot-notation should always be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

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

##Spacing
* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in IDE ( Xcode/Appcode ).

##Conditionals
* Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.
* should always use braces

**Preferred:**
```objc
if (condition) {
    // do stuff
} else {
    // do other stuff
}
```

##Ternary Operator
**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```
**Not Preferred:**
```objc
result = a ]]> b ? x = c ]]> d ? c : d : y;
```

##Private Properties/Methods
* Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as RWTPrivate or private) should never be used unless extending another class. The Anonymous category can be shared/exposed for testing using the +Private.h file naming convention.

**Preferred:**
```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

##Organization
* use `#pragma mark -`s to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

**Preferred:**
```objc
#pragma mark - Lifecycle
+ (instancetype)objectWithThing:(id)thing {}
- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)property {}
- (id)anotherCustomProperty {}

#pragma mark - Actions

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

##Singletons

**Preferred:**

```objc
+ (instancetype)sharedInstance {  static id sharedInstance = nil;
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
  });
  return sharedInstance;
}
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

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

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**Preferred:**
```objc
static NSString * const VSCAboutViewControllerCompanyName = @"voyages-sncf.com";

static CGFloat const VSCImageThumbnailHeight = 50.0;
```

**Not Preferred:**
```objc
#define CompanyName @"voyages-sncf.com"

#define thumbnailHeight 2
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

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


## Init Methods

Init methods should follow the convention provided by Apple's generated code template.  A return type of 'instancetype' should also be used instead of 'id'.

[More information from Apple doc: Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```


## Dealloc
Dealloc methods are no longer required when using arc but in certain cases must be used to remove observers, KVO, etc.

**Preferred:**
```objc
- (void)dealloc{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```



## Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

[More information about boolean](http://nshipster.com/bool/)

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

If the name of a `BOOL` property is expressed as an adjective, the property can omit the â€œisâ€ prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```



## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`
[More information from Apple doc: Adopting Modern Objective-c](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)
**For Example:**

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant definition):

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```
Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

## Golden Path

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

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

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


##Basic Code Principles

* Each function/method should aim to perform one action/task that reflects it's name.
* Since each function/method performs one action/task each one should be relatively short. If the code does not fit on one screen for example, you know you have a problem!
* Declare local variables as close to the code they are used in as possible.
* Always aim to reduce code nesting (ie a statement that is nested in an if, an if, a for and an if is hard for the brain to evaluate. Refactor!).

