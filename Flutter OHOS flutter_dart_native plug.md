
# DartNative

DartNative serves as a bridge between Dart and native APIs, providing a faster and more concise alternative to the lower-performance Flutter Channel.

## Features

### Dynamic Synchronous & Asynchronous Channels
- Dynamically invoke any native API
- Supports both synchronous and asynchronous communication channels

### Direct Multi-Language Interface Calls
- Eliminates parameter and return value serialization required by Flutter Channel
- Provides direct interface calls between languages
- Automates object marshaling

### Dart Finalizer Support
- Supports Dart finalizer functionality on Flutter 2.2.0 (Dart 2.13.0) and higher
- Works without requiring Flutter 3 (Dart 2.17+) like native Dart finalizer

### Automatic Bridge Code Generation
- Automatic type conversion reduces bridge code complexity
- Generates cleaner and shorter bridge code than Flutter Channel

Design Philosophy and Vision:


## Requirements

	| DartNative Version | Flutter Requirement                  | Code Generation Version |
	|--------------------|--------------------------------------|-------------------------|
	| 0.4.x - 0.7.x     | Flutter 2.2.0 (Dart 2.13.0)          | 2.x                     |
	| 0.3.x              | Flutter 1.20.0 (Dart 2.9.1)          | 1.2.x                   |
	| 0.2.x              | Flutter 1.12.13 (Dart 2.7)           | 1.x                     |

## Supported Platforms
iOS, macOS, and Android

## Usage

### Basic Usage: Interface Binding
Add `dart_native` to dependencies and `build_runner` to dev_dependencies. Example usage:

#### Dart Calling Native
**Dart Code:**

    ```dart
    final interface = Interface("MyFirstInterface");
    
    // String type example
    String helloWorld() {
      return interface.invokeMethodSync('hello', args: ['world']);
    }
    
    // Numeric type example
    Future<int> sum(int a, int b) {
      return interface.invokeMethod('sum', args: [a, b]);
    }
    ```

**Objective-C Equivalent:**
    ```objective-c
    @implementation DNInterfaceDemo
    
    // Register interface name
    InterfaceEntry(MyFirstInterface)
    
    // Register "hello" method
    InterfaceMethod(hello, myHello:(NSString *)str) {
      return [NSString stringWithFormat:@"hello %@!", str];
    }
    
    // Register "sum" method
    InterfaceMethod(sum, addA:(int32_t)a withB:(int32_t)b) {
      return @(a + b);
    }
    
    @end
    ```

**Java Equivalent:**

    ```java
    // Load libdart_native.so
    DartNativePlugin.loadSo();
    
    @InterfaceEntry(name = "MyFirstInterface")
    public class InterfaceDemo extends DartNativeInterface {
    
      @InterfaceMethod(name = "hello")
      public String hello(String str) {
    return "hello " + str;
      }
    
      @InterfaceMethod(name = "sum")
      public int sum(int a, int b) {
    return a + b;
      }
    }
    ```

> **Note:** For custom paths:  
> - Java: `DartNativePlugin.loadSoWithCustomPath("xxx/libdart_native.so")`  
> - Dart: Call `dartNativeInitCustomSoPath()` before using DartNative

#### Native Calling Dart
**Dart Code:**
    
    ```dart
    interface.setMethodCallHandler('totalCost',
      (double unitCost, int count, List list) async {
    return {'totalCost: ${unitCost * count}': list};
      });
    ```

**Objective-C Equivalent:**
    
    ```objective-c
    [self invokeMethod:@"totalCost"
       arguments:@[@0.123456789, @10, @[@"testArray"]]
      result:^(id _Nullable result, NSError * _Nullable error) {
      NSLog(@"%@", result);
    }];
```

**Java Equivalent:**

    ```java
    invokeMethod("totalCost", new Object[]{0.123456789, 10, Arrays.asList("hello", "world")},
      new DartNativeResult() {
    @Override
    public void onResult(@Nullable Object result) {
      Map retMap = (Map) result;
      // Handle result
    }
    
    @Override
    public void error(@Nullable String errorMessage) {
      // Handle error
    }
      }
    );
    ```

#### Dart Finalizer

    ```dart
    final foo = Bar(); // Custom instance
    unitTest.addFinalizer(() {
      print('The \'foo\' instance has been destroyed!');
    });
    ```

### Advanced Usage: Dynamic Method Invocation

1. **Add Dependencies**:  
   Add `dart_native` to dependencies and `build_runner` to dev_dependencies

2. **Code Generation**:  
   Use `@dartnative/codegen` to generate Dart wrapper code or write manually

3. **Generate Type Conversion Code**:  
   - Annotate wrapper class with `@native`:
   
     ```dart
     @native
     class RuntimeSon extends RuntimeStub {
       RuntimeSon([Class isa]) : super(Class('RuntimeSon'));
       RuntimeSon.fromPointer(Pointer<Void> ptr) : super.fromPointer(ptr);
     }
     ```
   - Annotate entry point (e.g., `main()`) with `@nativeRoot`:
   
         ```dart
     @nativeRoot
     void main() {
       runApp(App());
     }
     ```

   - Run code generation:
     ```
     flutter packages pub run build_runner build --delete-conflicting-outputs
     ```
     > **Tip:** Clean first with:  
     > `flutter packages pub run build_runner clean`

4. **Call Generated Functions**:  
   Function names are determined by `name` in `pubspec.yaml`:
    
       ```dart
       @nativeRoot
       void main() {
     runDartNativeExample(); // Generated function
     runApp(App());
       }
       ```

5. **Write Platform-Specific Code**:

#### iOS Example
**Dart Code (Generated):**

    ```dart
    // Create Objective-C object
    RuntimeStub stub = RuntimeStub();
    
    // Convert Dart function to Objective-C block
    stub.fooBlock((NSObject a) {
      print('hello block! ${a.toString()}');
      return 101;
    });
    
    // Support built-in structs
    CGRect rect = stub.fooCGRect(CGRect(4, 3, 2, 1));
    print(rect);
    ```

**Objective-C Equivalent:**

    ```objective-c
    typedef int(^BarBlock)(NSObject *a);
    
    @interface RuntimeStub
    - (CGRect)fooCGRect:(CGRect)rect;
    - (void)fooBlock:(BarBlock)block;
    @end
    ```

> More iOS examples: [ios_unit_test.dart](/dart_native/example/lib/ios/unit_test.dart)

#### Android Example
**Dart Code (Generated):**

    ```dart
    // Create Java object
    RuntimeStub stub = RuntimeStub();
    
    // Get Java list
    List list = stub.getList([1, 2, 3, 4]);
    
    // Support interfaces
    stub.setDelegateListener(DelegateStub());
    ```

**Java Equivalent:**

    ```java
    public class RuntimeStub {
      public List<Integer> getList(List<Integer> list) {
    List<Integer> returnList = new ArrayList<>();
    returnList.add(1);
    returnList.add(2);
    return returnList;
      }
    
      public void setDelegateListener(SampleDelegate delegate) {
    delegate.callbackInt(1);
      }
    }
    ```

> More Android examples: [android_unit_test.dart](https://gitee.com/openharmony-sig/flutter_dart_native/blob/master/dart_native/example/lib/android/unit_test.dart)

> **macOS Note**: Add `use_frameworks!` to your Podfile when using DartNative on macOS
