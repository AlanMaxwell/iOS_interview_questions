

## 1. Give me a short brief about yourself
I'm a software developer with N years of experience. For iOS I made some projects that included BLE, SwiftUI, network protocols (if you used them), etc.

## 2. Can you describe some of your projects and your role in these projects?

## 3. What frameworks you used in your iOS projects?
You can specify frameworks like:
UIKit, SwiftUI, Combine, AVFramework, PushNotification, CallKit, GCD, AsyncSocket, Core Bluetooth, etc.

## 4. How the Optional is implemented in Swift?
```
enum Optional<Wrapped>
{ 
  case none 
  case some(Wrapped) 
}
```
## 5. Is there a difference between .none and nil?

No

## 6. Ways to unwrap optional variables
```
var a:Int?

a! //1

if let s = a { //2

}

if let a { //3

}

guard let s = a else { //4

}

a ?? 0 //5
```

## 7. What's difference between reference and value types? What reference and value types you know?

value types:
structs, enums, arrays, dictionaries, strings

reference types:
classes, closures, NS types are all classes (NSString, for example)

Two differences between value and reference types:  
1) Value types are stored in Stack and reference types are stored in Heap.
2) If you assign one object to another for reference type, you just copy the reference, not value:

```
// Reference type example
class C { var data: Int = -1 }
var x = C()
var y = x						// x is copied to y
x.data = 42						// changes the instance referred to by x (and y)
println("\(x.data), \(y.data)")	// prints "42, 42"
```

For value types you will copy the value of the variable.

## 8. What's difference between Class and Structure?

* Structures are value types.
* Classes are reference types.
* Structures don’t support inheritance.
* classes support inheritance.
* Structures don’t support de-initializers. ( deinit )
* Classes support deinitializers.
* Structures don’t follow Reference Counting.
* Classes follow Reference Counting.
* Mutating Keyword is needed to modify the property values in Structure’s instance methods.
* No need of mutating keyword to modify the class variable’s value.


## 9. Do you know what copy-on-write means?
If you assign one array to another (not only array, there are other objects), then the second object will refer to the first array address until the second array is not changed
```
func addressOf(_ o: UnsafeRawPointer) -> String {
    let addr = unsafeBitCast(o, to: Int.self)
    return String(format: "%p", addr)
}


var array = [1, 2, 3, 4, 5]
addressOf(array) // 0x600002e30ac0

var array2 = array
addressOf(array2) // 0x600002e30ac0

array.append(6)
addressOf(array2) // 0x6000026119f0
```

## 10. Do you know what are SOLID principles?

SOLID:

### S – single responsibility principle. 
It’s when a class has just one purpose. A class shouldn’t contain functions that could be moved to other classes

### O – open/closed principle (OCP)
A class should be opened for extension, but closed for changes.

Open closed principle allows to avoid this kind of code: 
```
protocol SomeProtocol {
}

class A:SomeProtocol {
    func printClassAName() {
        print("I'm A")
    }
}


class B:SomeProtocol{
    func printClassBName() {
        print("I'm B")
    }
}

class Caller {
    func printClassName(obj:SomeProtocol){
    ////TO AVOID THIS KIND OF CODE!!!!!
        if let unwrappeObj = obj as? A {
            obj.printClassAName()
        }
        else if let unwrappeObj = obj as? B {
            obj.printClassBName()
        }
    
    }
}
```

It should be changed like that to avoid changes in Caller class in the future:
```
protocol SomeProtocol {
  func printClassName()
}

class A:SomeProtocol {
    func printClassName() {
        print("I'm A")
    }
}


class B:SomeProtocol{
    func printClassName() {
        print("I'm B")
    }
}

class Caller {
    func doSomething(obj:SomeProtocol){
        print(obj.printClassName())
    }
}
```
This principle is similar with D (dependency inversion principle), but OCP is more general. The DIP is an extension of the OCP.


### L – Liskov principle. 
In simple words this principle says that you need to have a possibility to use a parent and a child classes without any difference.
```
Class A: SomeProtocol {

}

Class B: SomeProtocol {

}
let a = A()
let b = B()

var a:[SomeProtocol] = [a, b]
```
### I – interface segregation. 
Classes SHOULDN'T implement protocol methods they don’t use.
If we noticed this situation, just move these methods to a separate protocol.

### D – dependency inversion. 
It means that a class shouldn’t depend on low-level modules – they both should depend on an Abstraction

For example:
```
class FileSystemManager {
    func save(string: String) {
        // Open a file
        // Save the string in this file
        // Close the file
   }
}
class Handler {
    let fileManager = FilesystemManager()
    func handle(string: String) {
        fileManager.save(string: string)
    }
}
```

If in the future we’ll need to add other methods for saving data (data base, for example), we should inherit FilesystemManager from some Storage protocol and use it instead of FilesystemManager and other possible data saving ways
```
class FileSystemManager:Storage {
    func save(string: String) {
        // Open a file
        // Save the string in this file
        // Close the file
   }
}

class DataBaseManager:Storage {
    func save(string: String) {
        // Open DB
        // Save the data
        // Close DB
   }
}

class Handler {
    let storage:Storage
    func handle(string: String) {
        storage.save(string: string)
    }
}
```

This principle is similar with OCP is more general. The DIP is an extension of the OCP.

Difference is that OCP is for similar functions, but DIP deals with the same input data


## 11. What is Singleton?

The main point of Singleton is to ensure that we initialized something only once and this "something" should be available from everywhere. For example, UIApplication.shared

P.S.: ServiceLocator – is a singleton with an array of some services


## 12. How are you doing your code reviews?

The best practice said that the code review should depend on CI/CD tests, a style guide, SOLID, and some linter (a syntax checker)

## 13. Application lifecycle

Use this:

![Application Lifecycle](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/appLifecycle.png)

## 14. ViewController lifecycle

    • ViewDidLoad - Called when you create the class and load from xib. Great for initial setup and one-time-only work.
    • ViewWillAppear - Called right before your view appears, good for hiding/showing fields or any operations that you want to happen every time before the view is visible. Because you might be going back and forth between views, this will be called every time your view is about to appear on the screen.
    • ViewDidAppear - Called after the view appears - great place to start an animations or the loading of external data from an API.
    • ViewWillDisappear/DidDisappear - Same idea as ViewWillAppear/ViewDidAppear.
    • ViewDidUnload/ViewDidDispose - In Objective-C, this is where you do your clean-up and release of stuff, but this is handled automatically so not much you really need to do here

![ViewController lifecycle](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/viewControllerLifecycle.png)


## // TODO: Can you say what ViewController lifecycle methods are calling when you started to segue from a view (A) to another view (B), but haven't finish it?

## 15. What architecture patterns you used?

Better to mention MVC, MVVM, VIPER

## 16. What is VIPER?

VIPER – is an architecture pattern with these parts:

* R – router – an entry point
* E – entity – model (like in MVC, for example)
* P – presenter – holds the reference to interactor, to a router and to a view
* Presenter uses data, received using fetching data functions from Interactor to update View.
* V – view. But with additional protocol with updating functions to call. For example, if we want to show an alert in a view, then we should ask the presenter to do that
* I – Interactor handles business logic and data retrieval

## 17. What is MVVM?
MVVM - Model View ViewModel

Model - is the data layer
View - is a view
ViewModel - contains presentation logic (process data from Model to View, reacts on actions from the View and transfers these reactions to Model).

In the code tree there should be three different directories: Models, Views, ViewModels. Each of your classes should be represented separately there.

Sometimes it's wise to create an additional directory called Services, in which you can put your business logic. 

## TODO: What is MVVM-C?

## 18. Who is an owner of data in MVVM?

Model is the data itself.

## 19. MVC MVVM differences:

The main difference between MVC and MVVM is the role of the controller and view model. 
In MVC, the Controller handles user input and updates the Model and View. In MVVM, the VIEW MODEL handles user input and updates the Model and View, and the view is responsible for displaying the data.

## 20. What NS prefix means in some Swift and Objective-C classes?
It means next step (the name of one company)

## 21. How memory management works in iOS? (Automatic reference counter (ARC))

In this question it's better to tell about ARC and retain cycles.

Short explanation:
ARC automatically keeps track of the number of references to an object, and when that number reaches zero, it deallocates the object
Counter decreases after an object releases
Any object deallocates after the counter is 0

If two objects have a strong reference to each other – retain cycle. Use weak or unowned to avoid that

(weak variables are presented as optional if to take a look on its type)
(unowned variables are presented as usual (they can’t be nil))

My advise is to watch this video from Apple:
https://developer.apple.com/videos/play/wwdc2021/10216/

## 22. What is map, flatMap, compatMap, reduce. Difference between map, flatMap, compatMap

### Mathematically:

map:
```
var arr:[Int] = [1, 2, 3]
arr = arr.map {
  return $0+1
}
```

flatMap (is equivalent to Array(s.map(transformation).joined())):
```
var arr:[Int] = [[1,2,3],[4,5,6]]
arr = arr.flatMap {
  return $0
}
```

compatMap - same as map, but filters nil values

### In Combine

map is used to transform each value emitted by a publisher using a provided closure. The closure takes in a value emitted by the publisher and returns a new value.

compatMap is similar to map, but it also filters out any values that are nil before emitting the new values

flatMap returns a new publisher with an emitted value as a parameter:
```
private var imagesListSubject = PassthroughSubject<String, Error>()

imagesListSubject
   .removeDuplicates()
   .flatMap { [unowned self] day in
        self.networkService.fetchDayImagesList(day: day)
   }
```

## 23. How to make a multilevel dismiss in SwiftUI? (to dismiss multiple level navigation)
1) You can use @EnvironmentObject, because it's available in all nested views
2) You can transfer @Binding variable from the root view to new navigation levels and if you need, just toggle this variable
3) Use an architecture pattern in which you can just set a current view. Like VIPER, REDUX and Composable architecture


## 24. What is a View protocol in swiftUI?
In SwiftUI, the View protocol is the fundamental building block of layout and user interface. But without some content it can't exist


## 25. Why Views are structures in SwiftUI?
   * structs are simpler to work with and faster than classes
   * it takes less memory (it takes only what was set, without multilevel inheritance)
   * views that don’t mutate over time

## 26. Is there a way to use UIKit elements in SwiftUI?

Yes. You should create a class that conforms to UIViewRepresentable and UIViewControllerRepresentable protocols.

But this is a long story. If you are curious in implementation, just try to find a couple of examples.


## 27. Redux in iOS (example of button tapping)

A simple example in which a button increments a counter.
```
// The state of the application
struct AppState {
    var count: Int = 0
}

// The actions that can be dispatched to the store
enum CounterAction: Action {
    case increment
    case decrement
}

// The reducer, which handles the actions and updates the state
func counterReducer(action: Action, state: AppState?) -> AppState {
    var state = state ?? AppState()

    switch action {
    case let action as CounterAction:
        switch action {
        case .increment:
            state.count += 1
        case .decrement:
            state.count -= 1
        }
    default:
        break
    }

    return state
}

// The store, which holds the state and handles the actions
let store = Store<AppState>(
    reducer: counterReducer,
    state: nil
)

// The view, which displays the state and dispatches actions
struct ContentView: View {
    @ObservedObject var store: Store<AppState>

    var body: some View {
        VStack {
            Text("Count: \(store.state.count)")
            Button("Increment") {
                self.store.dispatch(action: CounterAction.increment)
            }
        }
    }
}
```

## 27. Composable architecture
* View sends events to Action
* Action sends an action to a Reducer (keeps state of the app alive)
* Reduces mutate State, sends a call to Effect (outside world), interacts with Environment (dependencies, that are helpful for testing)
* State influence View

## 28. What asynchronous functionality is available in Swift?

* DispatchQueue
* DispatchGroup
* Operation queues
* delegate events
* Timer operations
* Combine publisher that is sending data
* async/await

A note: you should always update interface only on main thread (DispatchQueue.main), otherwise it can just stuck

## 29. What HTTP methods you know?

* HTTP:
* POST: Sends data to specific server to create or update information.
* PUT: Sends data to specific server to create or update information without the risk of creating the resource more than once.
* HEADER: Previews what the GET request response might be without the body of the text.
* OPTIONS: Learns the communication channels used by the target source.
* GET: Requests information from a specific source.
* DELETE: Removes information.

            
## 30. How do you test network calls in Unit test?

### 1)You should mock network calls.
```
protocol NetworkServiceProtocol {
    func getDataFromServer(completion: @escaping (Result<Data, Error>) -> Void)
}
```
Then, create a mock implementation of the protocol that returns pre-defined data:

```
class MockNetworkService: NetworkServiceProtocol {
    func getDataFromServer(completion: @escaping (Result<Data, Error>) -> Void) {
        let data = Data("Mocked data".utf8)
        completion(.success(data))
    }
}
```
Now, in your test case, you can inject the mock network service into your code:

```
func testGetDataFromServer() {
    let mockService = MockNetworkService()
    let viewModel = MyViewModel(networkService: mockService)
    viewModel.getDataFromServer()

    // Assert that the view model processed the mocked data correctly
    XCTAssertEqual(viewModel.result, "Mocked data")
}
```

### 2) You can mock not only network calls, you can mock entire classes using, for example, OCMock framework

## 3) Apple recommends to do something like this, to mock URLSession configuration

![URLSessionMock](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/URLSessionMock.png)

https://developer.apple.com/videos/play/wwdc2018/417/


## 32. How do you make branches?
For each feature create a separate branch, launch CI/CD, merge to develop. When release is coming, merge develop to master.


## 33. What was your biggest challenge as an iOS developer?

Think about it, I'm sure you can find some accomplishment that you're proud of.


## 34. What you can bring to the company as an iOS developer?

## 35. Did you work with the team? Describe your usual working process

## 36. Do you have some questions about the company?

Here it's better to ask about the project if they still didn't tell that, what is the earliest iOS version in the project, what management system they use, does they provide some budget for learning purposes (for example, paid courses compensation)


## P.S.: Don't forget to call the interviewer by name, people like it

## A small note: if you made a mistake during your life coding then you completely failed the interview.
Companies and trainings always tell that if you don’t know how to solve a problem or answer on a specific technical question then it doesn't mean that you failed the interview. Actually it does, but they always deny that.
There is always another candidate who make the task without a problem, so if you will fail something - you failed. So prepare harder, practice again and again.


