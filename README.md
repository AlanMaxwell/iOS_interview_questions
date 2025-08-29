
# Technical

## 1. What frameworks you used in your iOS projects?
You can specify frameworks like:Cancel changes
UIKit, SwiftUI, Combine, AVFramework, PushNotification, CallKit, GCD, Core Bluetooth, etc.

## 2. How the Optional is implemented in Swift?
```
enum Optional<Wrapped>
{ 
  case none 
  case some(Wrapped) 
}
```
## 3. Is there a difference between .none and nil?

No

## 4. Ways to unwrap optional variables
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

## 5. What's difference between reference and value types? What reference and value types you know?

**Value types**:
structs, enums, arrays, dictionaries, strings

**Reference types**:
classes, closures, NS types are all classes (NSString, for example), actors

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

## 6. What's difference between Class and Structure?

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


## 7. Do you know what copy-on-write means?
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

array2.append(6)
addressOf(array2) // 0x6000026119f0
```

### How it's implemented?
A type contains isKnownUniquelyReferenced() method, which checks if the current instance is uniquely referenced or if a copy needs to be made.

## 8. Do you know what are SOLID principles?

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

### 8.1 What SOLID principles Apple broke in their practice?

Apple broke open-closed principle when moved to newer Swift versions and single responsibility, because viewcontroller contains presentation logic and presentation layer inside.


## 9. What is Singleton?

The main point of Singleton is to ensure that we initialized something only once and this "something" should be available from everywhere. For example, UIApplication.shared

P.S.: ServiceLocator – is a singleton or an injected object with an array of some services.


## 10. How are you doing your code reviews?

The best practice said that the code review should depend on CI/CD tests, a style guide, SOLID, and some linter (a syntax checker)

## 11. Application lifecycle

Use this:

![Application Lifecycle](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/appLifecycle.png)

## 12. ViewController lifecycle

* ViewDidLoad - Called when you create the class and load from xib. Great for initial setup and one-time-only work.
* ViewWillAppear - Called right before your view appears, good for hiding/showing fields or any operations that you want to happen every time before the view is visible. Because you might be going back and forth between views, this will be called every time your view is about to appear on the screen.
* ViewDidAppear - Called after the view appears - great place to start an animations or the loading of external data from an API.
* ViewWillDisappear/DidDisappear - Same idea as ViewWillAppear/ViewDidAppear.
* ViewDidUnload/ViewDidDispose - In Objective-C, this is where you do your clean-up and release of stuff, but this is handled automatically so not much you really need to do here

![ViewController lifecycle](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/viewControllerLifecycle.png)

### P.S.: View lifecycle in SwiftUI: Appearing, Updating, Disappearing

## // TODO: Can you say what ViewController lifecycle methods are calling when you started to segue from a view (A) to another view (B), but haven't finish it?

## 13. What architecture patterns you used?

Better to mention MVC, MVVM, VIPER

## 14. What is VIPER?

VIPER – is an architecture pattern with these parts:

* R – router – an entry point
* E – entity – model (like in MVC, for example)
* P – presenter – holds the reference to interactor, to a router and to a view
* Presenter uses data, received using fetching data functions from Interactor to update View.
* V – view. But with additional protocol with updating functions to call. For example, if we want to show an alert in a view, then we should ask the presenter to do that
* I – Interactor handles business logic and data retrieval

## 15. What is MVVM?
MVVM - Model View ViewModel

* **Model** - is the data layer.
* **View** - is a view.
* **ViewModel** - contains presentation logic (process data from Model to View, reacts on actions from the View and transfers these reactions to Model).

In the code tree there should be three different directories: Models, Views, ViewModels. Each of your classes should be represented separately there.

Sometimes it's wise to create an additional directory called Services, in which you can put your business logic. 

## 16. What is MVVM-C?
It is MVVM with Coordinator, where Coordinator is an object (in SwiftUI it's an EnviromnentObject), that is common for all Views, that is responsible for navigation. It switches views depending on some observable variable, usually Enum.


## 17. Who is an owner of data in MVVM?

Model is the data itself.

## 18. MVC MVVM differences:

The main difference between MVC and MVVM is the role of the controller and view model. 
In MVC, the Controller handles user input and updates the Model and View. In MVVM, the VIEW MODEL handles user input and updates the Model and View, and the view is responsible for displaying the data.

## 19. What NS prefix means in some Swift and Objective-C classes?
It means next step (the name of one company)

## 20. How memory management works in iOS? (Automatic reference counter (ARC))

In this question it's better to tell about ARC and retain cycles.

Short explanation:
ARC automatically keeps track of the number of references to an object, and when that number reaches zero, it deallocates the object.
Counter decreases after an object releases.
Any object deallocates after the counter is 0.

If two objects have a strong reference to each other – retain cycle. Use weak or unowned to avoid that.

My advise is to watch this video from Apple:
https://developer.apple.com/videos/play/wwdc2021/10216/

Weak links also considered, but in a separate weak references counter.

### 20.1 On what thread memory deallocates?
On the main thread

### 20.2 What's difference between unowned and weak

Weak variables are presented as optional if to take a look on its type
Unowned variables are presented as usual (they can’t be nil))

Unowned works faster, because there is no optional chech, but it's less safer (if the object doesn't exist, we'll have a crash). So we can use it if we're sure that the object will not be nil or when performance is critical.

## 21. What is map, flatMap, compatMap, reduce. Difference between map, flatMap, compatMap

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

## 21. What's difference between bounds and frame in UIView?

The difference in coordinate system. Bounds refers to its coordinates relative to its own space (as if the rest of your view hierarchy didn’t exist), frame refers to its coordinates relative to its parent’s space.

## 22. How to make a multilevel dismiss in SwiftUI? (to dismiss multiple level navigation)
1) You can use @EnvironmentObject, because it's available in all nested views
2) You can transfer @Binding variable from the root view to new navigation levels and if you need, just toggle this variable
3) Use an architecture pattern in which you can just set a current view. Like VIPER, REDUX and Composable architecture


## 23. What is a View protocol in SwiftUI?
In SwiftUI, the View protocol is the fundamental building block of layout and user interface. But without some content it can't exist


## 24. Why Views are structures in SwiftUI?
   * structs are simpler to work with and faster than classes
   * it takes less memory (it takes only what was set, without multilevel inheritance)
   * views that don’t mutate over time

## 25. Is there a way to use UIKit elements in SwiftUI?

Yes. You should create a class that conforms to UIViewRepresentable and UIViewControllerRepresentable protocols.

But this is a long story. If you are curious in implementation, just try to find a couple of examples.


## 26. Redux in iOS (example of button tapping)

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
* Push Notifications
* Calendar Operations
* Combine publisher that is sending data
* async/await
* Tasks - that are using with async/await
* AsyncSequence
* background tasks (separate mechanism that is useful when you run tasks for an application in background). It runs not so long, but sometimes it's useful if you wake up the application in background and want to do something for this period.

A note: you should always update interface only on main thread (DispatchQueue.main), otherwise it can just stuck

## 29. What's difference between DispatchQueue, Operation queues, async/await?

* Difference between DispatchQueue and async/await is in tasks scheduling. DispatchQueue create a separate thread for each waiting task, whereas async/await is trying to launch everything on just one thread by suspending waiting functions. So async/await reduces number of threads and the number of threads switching.
https://developer.apple.com/videos/play/wwdc2021/10254/
* async/await is the language feature, DispatchQueue is a framework.
* DispatchQueue and OperationQueue are used for managing work on threads, async/await is used for writing asynchronous code in a synchronous style.

* DispatchQueue and OperationQueue difference - you can't cancel DispatchQueue tasks. So, DispatchQueue are better for short tasks that should have minimum performance and memory, OperationQueue are more suitable for long-running operations that may need to be cancelled or have complex dependencies


## 30. What is a dead lock?

It's when two different threads want to access to a shared resource, but they are both waiting for each other to unlock.

## 31. What is a race condition?

It's when two different threads write and read from a shared resource without syncronization. It leads to broked data.

## 32. How to launch a group of asyncronous tasks?

If the order doesn't matter, you can just use DispatchQueue.main.async (or DispatchQueue.global.async).

If order matters you can use DispatchGroup with DispatchQueue or Operation Queue. But what to choose depends on a task specific: when you use Operation Queue you will not be notified when these tasks are done, but in DispatchGroup you will be. And if you use DispatchGroup you can't cancel tasks execution, but for Operation Queue you can.

Also, using Combine you can launch several tasks using "CombineLatest"

## 33. Can a DispatchQueue task be cancelled?

No. But DispatchWorkItem can be cancelled.

## 34. What HTTP methods you know?

* HTTP:
* POST: Sends data to specific server to create or update information.
* PUT: Sends data to specific server to create or update information without the risk of creating the resource more than once.
* HEADER: Previews what the GET request response might be without the body of the text.
* OPTIONS: Learns the communication channels used by the target source.
* GET: Requests information from a specific source.
* DELETE: Removes information.

            
## 35. How do you test network calls in Unit test?

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

### 3) Apple recommends to do something like this, to mock URLSession configuration

![URLSessionMock](https://github.com/AlanMaxwell/iOS_interview_questions/blob/main/URLSessionMock.png)

https://developer.apple.com/videos/play/wwdc2018/417/

Here is the code example of mocked HTTP request using Combine:

####
```
import XCTest

class MockURLProtocol: URLProtocol {
    static var requestHandler: ((URLRequest) throws -> (HTTPURLResponse, Data))?
    
    override class func canInit(with request: URLRequest) -> Bool {
        return true
    }
    
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        return request
    }
    
    override func startLoading() {
        guard let handler = MockURLProtocol.requestHandler else {
            XCTFail("Received unexpected request with no handler set")
            return
        }
        do {
            let (response, data) = try handler(request)
            client?.urlProtocol(self, didReceive: response, cacheStoragePolicy: .notAllowed)
            client?.urlProtocol(self, didLoad: data)
            client?.urlProtocolDidFinishLoading(self)
        } catch {
            client?.urlProtocol(self, didFailWithError: error)
        }
    }
    
    override func stopLoading() {
    }
}

enum ServiceError: Error, Equatable {
    case invalidURL
    case noInternetConnection
    case requestTimeout
    case networkError
    case statusCodeError(code: Int?)
}

final class NetworkLayerTests: XCTestCase {

    var mockedUrlSession: URLSession!
    
    override func setUpWithError() throws {
        //exit test if something failes
        self.continueAfterFailure = false
        
        let configuration = URLSessionConfiguration.ephemeral
        
        //set up a mock for url session
        configuration.protocolClasses = [MockURLProtocol.self]
        mockedUrlSession = URLSession(configuration: configuration)
    }
    
    struct UserProfile:Codable {
        var name:String?
    }
    
    class ProfileAPI {
        let url = URL(string: "https://testURL.com/user")!
        private var cancellable: AnyCancellable?
        
        // session to be used to make the API call
        let session: URLSession
        
        // Make the session shared by default.
        // In unit tests, a mock session can be injected.
        init(urlSession: URLSession = .shared) {
            self.session = urlSession
        }
        
        // get user profile from backend
        func getProfile(completion: @escaping (UserProfile) -> Void) {
            cancellable = session.dataTaskPublisher(for: url)
                .mapError { error -> ServiceError in
                    switch error.code {
                    case .notConnectedToInternet:
                        return .noInternetConnection
                    case .timedOut:
                        return .requestTimeout
                    default:
                        return .networkError
                    }
                }
                .tryMap { data, response in
                    guard let httpResponse = response as? HTTPURLResponse,
                        200..<300 ~= httpResponse.statusCode else {
                        throw ServiceError.statusCodeError(code: (response as! HTTPURLResponse).statusCode)
                    }
                    return data
                }
                .decode(type: UserProfile.self, decoder: JSONDecoder())
                .receive(on: RunLoop.main)
                .catch { _ in Just(UserProfile()) }
                .sink { user in
                    completion(user)
            }
        }
    }
    
    func testCase() throws {
        
        let example = UserProfile(name: "Some User")
        let mockData = try JSONEncoder().encode(example)
        
        //set return data in mock request handler
        MockURLProtocol.requestHandler = { request in
            let response = HTTPURLResponse(url: URL(string: "https://someURL.com/test")!,
                                           statusCode: 200,
                                           httpVersion: nil,
                                           headerFields: ["Content-Type": "application/json"])!
            return (response, mockData)
        }

        //this is simpler example, but without http-status mocking
//        MockURLProtocol.requestHandler = { request in
//            return (HTTPURLResponse(), mockData)
//        }
        
        // Set expectation. Used to test async code.
        let expectation = XCTestExpectation(description: "response")
        
        // Make mock network request to get profile
        // here we use the previously set mocked UrlSession
        let profileAPI = ProfileAPI(urlSession: mockedUrlSession)
        
        profileAPI.getProfile { user in
            // Test
            XCTAssertEqual(user.name, "Some User")
            expectation.fulfill()
        }
        wait(for: [expectation], timeout: 1)
    }
}
```
## 36. What is the role of the "final" word in the class?
It prevents properties and functions from overriding.

## 37. What are lazy variables?
They initialize after the first time they are calling.

## 38. Pros and cons of using UIKit and SwiftUI

## 39. Is there a difference between "Codable" and "Encodable & Decodable" protocol inheritance?
No, Codable is a type alias for Encodable & Decodable

## 40. What are Encodable and Decodable protocols used for?

protocol Decodable : allows to decode bytes to the type that inherits Decodable

protocol Encodable : allows to represent a type as data bytes

Oftenly they are used for interaction with JSON and plist.

### P.S.: There could be a related question, can we rename keys during encoding/decoding?
Yes, we can using CodingKey syntax
```
struct Person: Decodable {
    var personName: String
    
    enum CodingKeys: String, CodingKey {
       case personName = "name"
    }
}
```

## 41. How to serialize a generic type?
Two ways:
1) Automatic: when you just inherit a type from Codable and just encode (P.S.: all class fields should conform to Encodable)
```
class Example<T: Codable>: Codable {
    let data: T
    
    init(data: T) {
        self.data = data
    }
}

let example = Example(data: ["John", "Doe", "john.doe@example.com"])
let encoder = JSONEncoder()
let data = try encoder.encode(example)
```
2) Manually - create a self written encode function. If you want to use, for example, not all fields
```
class Response<T: Encodable>: Encodable {
    let success: Bool
    let data: T
    
    let str:String = "" //extra field, which we don't use 

    
    init(success: Bool, data: T) {
        self.success = success
        self.data = data
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(success, forKey: .success)
        try container.encode(data, forKey: .data)
    }
    
    private enum CodingKeys: String, CodingKey {
        case success
        case data
    }
}
```

## 42. Hou would you explain App Transport Security (ATS) to a junior?
ATS blocks insecure URLSession connections. (Security criteria are shown here https://developer.apple.com/documentation/security/preventing_insecure_network_connections)

There are two ways to prevent connections blocking:
1) Set "Allow Arbitrary Loads" - it is not a good approach. It looks like that:
```
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
```

2) You can set the exceptions. That is the good approach. It looks like that:
```
<key>NSAppTransportSecurity</key>
    <dict>
      <key>NSAllowsArbitraryLoads</key>
      <false/>
      <key>NSExceptionDomains</key>
      <dict>
        <key>exception.com</key>
        <dict>
          <key>NSIncludesSubdomains</key>
          <true/>
          <key>NSExceptionAllowsInsecureHTTPLoads</key>
          <true/>
        </dict>
        
      </dict>
    </dict>
```


## 43. How would you explain Dependency Injection to a junior?
Dependency Injection is an approach, when functionality of one entity depends on the other entity and the FIRST gets the SECOND as a parameter.

I would provide an example of MVVM implementation, because link to ViewModel in View is a good example of Dependency Injection.

Why it's needed? To divide the functionality to different reusable parts and also to increase testability.

## 44. What three types of Dependency Injection you know?

* Constructor Injection: Dependency is passed to the object via its constructor. It used when the dependent object has to use the same concrete class for its lifetime.
* Method Injection: dependency is passed to the object via a method. It's useful if different objects could be injected. For example, if the dependency is a notification, sometimes this may sent as an SMS, and other times as an email for the same dependent object.
* Property Injection: If the dependency is selected and invoked at different places, we can set the dependency via a property exposed by the dependent object, which can then invoke it later

## 45. What's difference between @escaping and non-escaping closures?
@escaping launches after the function ends, non-escaping - just after its call.

## 46. What's difference between inout parameter of the function and a usual parameter?

```
func example(_ a: Int, _ b: inout Int) {}
```
inout keyword allows to change the value of the parameter variable.

## 47. In SwiftUI how to draw 1 million rows in a list?

1) You can use List view element
2) ScrollView + LazyVStack views


## 48. Do classes support multilple inheritance in Swift?

If to try to inherit from multiple classes, then NO. Swift classes don't support multiple inheritance.

But you can inherit multiple protocols.

## 49. How to prevent a class inheritance?

Using final keyword, like:
```
final class User {
    var name: String

    init(name: String) {
        self.name = name
    }
}
```

## 50. What is the default access level for class fields?
internal

## 51. What is id in Objective-C?
It is a generic type

## 52. What is responder chain?

The responder chain is responsible to handle all the events (like touch and motion) in iOS apps. If any responder can't handle a specific action or event, the event is sent to the next responder of the list to handle it until the list ends.

(conclusion taken from here: https://www.swiftanytime.com/blog/using-responders-and-the-responder-chain-to-handle-events)

It starts handling this event from UIApplication and finishes at the target element.

UIResponder abstract class responsible for some gesture events that we even can override.
To pass this event to other elements in the chain, these events contain also a reference to "next" element.

### 52. What is Hit test?
This method is called to determine which view should receive a touch event. By default, it returns the farthest descendant of the view hierarchy that contains a specified point.

Here is an example:
Imagine that you have two UIViews: a parent and a child. The child view is a subview of the parent view.
Imagine that we tap the child view, but we want the parent view to be tapped instead. How to reach this behavior?

You can achieve this behavior by overriding the hitTest(_:with:) method of the parent view to make the parent view receive touches that occur within the bounds of the child view:

```

class ParentView: UIView {
    override func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? {
        // Check if the point is inside the bounds of the parent view
        if self.bounds.contains(point) {
            // Return self (parent view) as the view to receive the touch
            return self
        }
        
        // If the point is not inside the parent view, let the default behavior handle it
        return super.hitTest(point, with: event)
    }
}

class ChildView: UIView {
    // Your implementation for the child view
}

// Usage
let parentView = ParentView()
let childView = ChildView()

parentView.addSubview(childView)
```
Now if we tap on the child view the parent view will be tapped instead

## 53. What is a zombie object?

It's an object, which is called after there is no more strong reference to this object.

## 54. What's difference between Any and AnyObject?

AnyObject refers to any instance of a class, Any - refers to any instance of any type: class, struct, enum, etc.

## 55. How to implement generic protocol?

Using associatedtype. 
```
protocol MyProtocol {
    associatedtype MyType
    func doSomething(with value: MyType)
}

class MyClass<T>: MyProtocol {
    typealias MyType = T
    
    func doSomething(with value: T) {
        // Implementation
        print("Doing something with \(value)")
    }
}
```
## 56. How to share common files between different applications on one device?

Using common App Groups for these applications.


## 57. What types of publishers Combine provides?

* Publishers.Sequence: The Sequence publisher emits a sequence of values provided as an array or a sequence type. It's commonly used to emit a predefined set of values.
* Publishers.Just: The Just publisher emits a single value and then finishes. It's similar to the Just publisher mentioned earlier but is a different implementation provided by the Combine framework.
* Publishers.Future: The Future publisher represents a result or value that will be available in the future. It's useful when you want to create a publisher that produces a single value at some point in time.
* Publishers.Timer: The Timer publisher emits a value after a specified time interval and then finishes. It's commonly used for creating time-based operations.

## 58. Why extension can't contain stored properties, but can contain computed variables?

Because of memory management. When you define a stored property its size calculates, but extensions developed like that so they should affect existing memory layout. Calculated variables don't affect.

## 59. What are hot and cold signals in reactive programming?

In reactive programming, the terms "cold" and "hot" signals refer to different ways of handling data streams.

   Cold Signals - they are data streams that start producing values only when a subscriber subscribes to them. Each subscriber receives its own independent stream of values.

   Better examples of cold signals include:
       Button taps: Each time a button is tapped, a new stream of events is created, and each subscriber receives the events starting from the moment they subscribed.
       Network requests: When making a network request, the response stream is typically cold. Each subscriber initiates its own network request and receives the response independently.

   Hot Signals - they are data streams that produce values regardless of whether there are active subscribers or not. Subscribers joining a hot signal receive only the values emitted after they subscribe. Hot signals share a single stream of values among multiple subscribers.

   Better examples of hot signals include:
       Notifications: System-wide notifications that can be observed by multiple components or modules in an application. Subscribers joining the notification stream receive only the notifications emitted after they subscribe.
       Sensor data: Continuous streams of data from sensors, such as GPS location updates or accelerometer readings, where the data is constantly being produced and can be observed by multiple parts of an application.

## 60. What SwiftUI property wrappers do you know?

You can list those:
* @State
* @Binding
* @ObservedObject
* @StateObject
* @EnvironmentObject - it is an observable object, which can be applied for several views and it affects these views simultaniously.

About others - it described in further questions.

## 61. What's difference between @ObservedObject and @StateObject?

@ObservedObject is used to keep track of an object that has already been created and passed as a parameter from the outside.
@StateObject is similar to @ObservedObject but it differf how SwiftUI manages their lifecycle.

SwiftUI is allowed to destroy a View in every moment. In this case if we created an @ObservedObject property, then it will be destroyed and recreated too.
@StateObject fixes that. If the View will be destroyed and recreated, then this object will REMAIN in memory and the View won’t lose the data during redrawing.
This works for child views, for example: if your child view has @StateObject property with some data, then if the parent redraws, then StateObject will not loose its value. But if the child view has an @ObservedObject property, then when the parent will be redrawn, then the child view will be redrawn too and @ObservedObject will be created again.


## 62. What's difference between @State and @Binding wrappers?

@State is meant for simple value type properties, which can be changed within a View. If to use it with reference types then its changes will not redraw data on the View.
@Binding is used to change properties bi-directionally. So, it can be used, for example, to change a property of a parent view from its child view.

## 63. What is Bytecode?

Middle code between the source code and machine code to allow to automatically compile an old application on the AppStore to new architectures.

## 64. What is Runloop?
It's an iOS system thread in which you can track states of other threads. For example, there you can make an ethernal check for some state.

## 65. What is dSYM?
It's a file with correspondence between source code and a binary of the application to make crash logs and debugging process readable.

## 66. What is autoreleasepool?

In Swift language it allows to not to wait until operating system cleans the memory during some actions (a loop, in a loop for example) and cleans memory as soon as possible.

## 67. Methods Dispatch. What types of it you know?

Static or Direct dispatch – fastest way of finding methods, because compiler knows method location from the beginning (scenario when a method hasn’t been overwritten (there is no inheritance)). So, value types have direct dispatch

Dynamic or Table dispatch – happens in reference types – a method is searched in the table of function pointers (witness table) and then it’s invokes. When we create a subclass, we make a copy of this witness table. If we override some method, we override it in this table.
It’s slower than Static, but allows inheritance

Message dispatch – more dynamic than Table dispatch. It’s changing at runtime. Swizzling is an example. And methods that are inherited from NSObject classes

How compiler choses the method?
For value types it’s always static, for classes and protocols it’s static in the extension and dynamic in the initial implementation. For final classes it’s still static. Dynamic and @objc
Keywords make it Message dispatch.

### If a class is not inherited, what dispatch it has?

### Methods dispatch for protocols and their extensions
For protocol id depends on type that implements it - value or reference. For extension static dispatch is used, because it calculates in compile time how much memory to allocate there.


## 68. What is "Hugging" and "Compression Resistance"?
Types of Constraints.

Hugging - content does not want to grow
Compression Resistance - content does not want to shrink

## 69. Semaphore vs Mutex

In swift Mutex is a lock, semaphore is a DispatchSemaphore

Mutex - at any point of time, only one thread can work with the entire buffer.
Semaphore - allows the access to one resource to several threads, depending on some preset maximum.

The Mutex is a locking mechanism - it blocks other threads from accessing some resource, and Semaphore is a signal mechanism which sends a signal when the resource is free and wait if it wants to access a resource.

## 70. DispatchSemaphore and DispatchGroup difference.
They are needed for different things. 
DispatchSemaphore restricts the number of threads which can access some resource
DispatchGroup – launches a group of tasks and waits until they end.

## 71. What is UIView, what is CALayer, who is responsible for what?

## 72. What's happening if we fill a large array with values/references?

If you create a very large array filled with reference types, allocating it on the stack can lead to a stack overflow, while allocating it on the heap can consume significant memory and may require proper memory management to prevent leaks

## 73. What's difference between as?, as!, as?

## 74. What is scene?

## 75. DispatchQueue qos. Explain usage of each qos priority

## 76. Design patterns. What categories you know? Provide examples of each category

## 77. Result type. Provide an example

# Behavioural: 

## 1. Give me a short brief about yourself
I'm a software developer with N years of experience. For iOS I made some projects that included BLE, SwiftUI, network protocols (if you used them), etc.

## 2. Can you describe some of your projects and your role in these projects?


## 3. How do you make branches?
For each feature create a separate branch, launch CI/CD, merge to develop. When release is coming, merge develop to master.

## 4. What was your biggest challenge as an iOS developer?

Think about it, I'm sure you can find some accomplishment that you're proud of.

## 5. What can you bring to the company as an iOS developer?

## 6. Did you work with the team? Describe your usual working process

## 7. Do you have some questions about the company?

Here it's better to ask about the project if they still didn't tell that, what is the earliest iOS version in the project, what management system they use, does they provide some budget for learning purposes (for example, paid courses compensation)


## P.S.: Don't forget to call the interviewer by name, people like it

## A small note: if you made a mistake during your life coding then you completely failed the interview.
Companies and trainings always say that if you don't know how to solve a problem or answer a specific technical question, it doesn't mean you failed the interview. Actually it does, but they always deny it.
There is always another candidate who can handle the task without any problem, so if you fail the task, you fail the interview. So prepare harder, practice again and again.

