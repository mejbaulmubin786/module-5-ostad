## 29. Behavioral Design Patterns
**Behavioral Design Patterns** হল ডিজাইন প্যাটার্নের একটি শ্রেণী যা অবজেক্টগুলির মধ্যে আচরণ, যোগাযোগ এবং সহযোগিতার প্যাটার্নগুলি নির্ধারণ করে। এগুলি প্রায়ই সফটওয়্যার সিস্টেমের বিভিন্ন অংশের মধ্যে কার্যকরী সম্পর্ক তৈরি করতে সাহায্য করে এবং কোডের নমনীয়তা ও পুনঃব্যবহারযোগ্যতা উন্নত করে।

### **Key Behavioral Design Patterns**

1. **Chain of Responsibility**: একটি অনুরোধ একাধিক অবজেক্টের মধ্যে প্রবাহিত হয়, যেখানে প্রতিটি অবজেক্ট নির্ধারণ করে যে এটি অনুরোধটি প্রক্রিয়া করবে কি না।

   - **Example**: একটি লগিং সিস্টেম যেখানে লগ বার্তা বিভিন্ন লেভেল (INFO, DEBUG, ERROR) দ্বারা প্রক্রিয়া করা হয়।

   ```php
   // Handler Interface
   interface Handler {
       public function setNext(Handler $handler);
       public function handleRequest($request);
   }

   // Concrete Handler
   class ConcreteHandlerA implements Handler {
       private $nextHandler;

       public function setNext(Handler $handler) {
           $this->nextHandler = $handler;
       }

       public function handleRequest($request) {
           if ($request == "A") {
               echo "Handled by ConcreteHandlerA\n";
           } elseif ($this->nextHandler) {
               $this->nextHandler->handleRequest($request);
           }
       }
   }

   // Usage
   $handlerA = new ConcreteHandlerA();
   $handlerB = new ConcreteHandlerB();
   $handlerA->setNext($handlerB);

   $handlerA->handleRequest("A");
   ```

2. **Command**: একটি রিকোয়েস্টকে একটি অবজেক্টে এনক্যাপসুলেট করে এবং এটি পরবর্তীতে প্রক্রিয়া করার জন্য সেই অবজেক্টটি পাস করে।

   - **Example**: একটি রিমোট কন্ট্রোল সিস্টেম যেখানে বিভিন্ন কমান্ড (লাইট অন/অফ) বিভিন্ন রিসিভার অবজেক্টের কাছে পাঠানো হয়।

   ```php
   // Command Interface
   interface Command {
       public function execute();
   }

   // Concrete Command
   class LightOnCommand implements Command {
       private $light;

       public function __construct(Light $light) {
           $this->light = $light;
       }

       public function execute() {
           $this->light->on();
       }
   }

   // Receiver
   class Light {
       public function on() {
           echo "Light is ON\n";
       }

       public function off() {
           echo "Light is OFF\n";
       }
   }

   // Usage
   $light = new Light();
   $lightOn = new LightOnCommand($light);
   $lightOn->execute();
   ```

3. **Iterator**: একটি সংগ্রহের উপাদানগুলির উপর পুনরাবৃত্তি করার জন্য একটি কনসিস্টেন্ট উপায় প্রদান করে, সংগ্রহের অভ্যন্তরীণ গঠন সম্পর্কে জানা ছাড়াই।

   - **Example**: একটি তালিকার উপাদানগুলির মধ্যে মাধ্যমে প্রবাহিত হওয়া।

   ```php
   // Iterator Interface
   interface Iterator {
       public function hasNext();
       public function next();
   }

   // Concrete Iterator
   class ConcreteIterator implements Iterator {
       private $items = [];
       private $position = 0;

       public function __construct(array $items) {
           $this->items = $items;
       }

       public function hasNext() {
           return $this->position < count($this->items);
       }

       public function next() {
           return $this->items[$this->position++];
       }
   }

   // Usage
   $items = ["item1", "item2", "item3"];
   $iterator = new ConcreteIterator($items);

   while ($iterator->hasNext()) {
       echo $iterator->next() . "\n";
   }
   ```

4. **Observer**: একটি অবজেক্ট (Subject) পরিবর্তিত হলে, অন্যান্য অবজেক্ট (Observers) স্বয়ংক্রিয়ভাবে আপডেট করা হয়।

   - **Example**: একটি সংবাদ সংস্থা যেখানে বিভিন্ন সাবস্ক্রাইবারদের নতুন খবর পাঠানো হয়।

   ```php
   // Observer Interface
   interface Observer {
       public function update($message);
   }

   // Subject
   class Subject {
       private $observers = [];

       public function addObserver(Observer $observer) {
           $this->observers[] = $observer;
       }

       public function notifyObservers($message) {
           foreach ($this->observers as $observer) {
               $observer->update($message);
           }
       }
   }

   // Concrete Observer
   class ConcreteObserver implements Observer {
       public function update($message) {
           echo "Received update: " . $message . "\n";
       }
   }

   // Usage
   $subject = new Subject();
   $observer = new ConcreteObserver();
   $subject->addObserver($observer);
   $subject->notifyObservers("New update available!");
   ```

5. **Strategy**: একটি পরিবারের এলগরিদমকে আলাদাভাবে এনক্যাপসুলেট করে এবং সেগুলির মধ্যে যেকোন একটি এলগরিদমের সাথে স্বচ্ছন্দে কাজ করতে সক্ষম করে।

   - **Example**: বিভিন্ন ধরনের সুরক্ষিত যোগাযোগ প্রোটোকল (TLS, SSL) সমর্থন করা।

   ```php
   // Strategy Interface
   interface Strategy {
       public function execute();
   }

   // Concrete Strategy
   class ConcreteStrategyA implements Strategy {
       public function execute() {
           echo "Strategy A\n";
       }
   }

   // Context
   class Context {
       private $strategy;

       public function __construct(Strategy $strategy) {
           $this->strategy = $strategy;
       }

       public function performOperation() {
           $this->strategy->execute();
       }
   }

   // Usage
   $strategy = new ConcreteStrategyA();
   $context = new Context($strategy);
   $context->performOperation();
   ```

6. **Template Method**: একটি অ্যালগরিদমের কাঠামো সংজ্ঞায়িত করে, তবে এক্সটেনশন দ্বারা কিছু পদক্ষেপ সাবক্লাস দ্বারা নির্ধারিত হয়।

   - **Example**: একটি প্রক্রিয়া যা কিছু ধাপের সাথে সংযুক্ত থাকে, কিন্তু নির্দিষ্ট ধাপগুলি সাবক্লাস দ্বারা বাস্তবায়িত হয়।

   ```php
   // Abstract Class
   abstract class AbstractClass {
       public function templateMethod() {
           $this->stepOne();
           $this->stepTwo();
       }

       abstract protected function stepOne();
       abstract protected function stepTwo();
   }

   // Concrete Class
   class ConcreteClass extends AbstractClass {
       protected function stepOne() {
           echo "Step One\n";
       }

       protected function stepTwo() {
           echo "Step Two\n";
       }
   }

   // Usage
   $concrete = new ConcreteClass();
   $concrete->templateMethod();
   ```

7. **State**: অবজেক্টের অভ্যন্তরীণ অবস্থার উপর ভিত্তি করে আচরণ পরিবর্তন করে।

   - **Example**: একটি ভেন্ডিং মেশিন যেখানে বিভিন্ন অবস্থার (অগ্রিম সঞ্চয়, সঠিক টাকা, শূন্য স্টক) উপর ভিত্তি করে কাজ পরিবর্তিত হয়।

   ```php
   // State Interface
   interface State {
       public function handle();
   }

   // Concrete States
   class ConcreteStateA implements State {
       public function handle() {
           echo "Handling State A\n";
       }
   }

   class ConcreteStateB implements State {
       public function handle() {
           echo "Handling State B\n";
       }
   }

   // Context
   class Context {
       private $state;

       public function __construct(State $state) {
           $this->state = $state;
       }

       public function setState(State $state) {
           $this->state = $state;
       }

       public function request() {
           $this->state->handle();
       }
   }

   // Usage
   $context = new Context(new ConcreteStateA());
   $context->request();
   $context->setState(new ConcreteStateB());
   $context->request();
   ```

8. **Mediator**: অবজেক্টগুলির মধ্যে যোগাযোগ নিয়ন্ত্রণ করে এবং তাদের মধ্যে সরাসরি যোগাযোগ কমায়।

   - **Example**: একটি চ্যাট সিস্টেম যেখানে ব্যবহারকারীদের বার্তাগুলি মধ্যস্থতার মাধ্যমে পাঠানো হয়।

   ```php
   // Mediator Interface
   interface Mediator {
       public function send($message, Colleague $colleague);
   }

   // Concrete Mediator
   class ConcreteMediator implements Mediator {
       private $colleague;

       public function setColleague(Colleague $colleague) {
           $this->colleague = $colleague;
       }

       public function send($message, Colleague $colleague) {
           echo "Message to colleague: " . $message . "\n";
       }
   }

   // Colleague
   abstract class Colleague {
       protected $mediator;

       public function __construct(Mediator $mediator) {
           $this->mediator = $mediator;
       }

       public function send($message) {
           $this->mediator->send($message, $this);
       }
   }

   // Concrete Colleague
   class ConcreteColleague extends Colleague {
       public function receive($message) {
           echo "Received message: " . $message . "\n";
      

 }
   }

   // Usage
   $mediator = new ConcreteMediator();
   $colleague = new ConcreteColleague($mediator);
   $mediator->setColleague($colleague);
   $colleague->send("Hello");
   ```

### **Summary**

**Behavioral Design Patterns** হল অবজেক্টগুলির আচরণ এবং সহযোগিতা নিয়ন্ত্রণ করার জন্য ডিজাইন প্যাটার্নের একটি সেট। এগুলি অন্তর্ভুক্ত করে:

- **Chain of Responsibility**
- **Command**
- **Iterator**
- **Observer**
- **Strategy**
- **Template Method**
- **State**
- **Mediator**

প্রতিটি প্যাটার্ন বিভিন্ন পরিস্থিতিতে ব্যবহার করা হয় এবং সফটওয়্যার ডিজাইন এবং উন্নয়ন প্রক্রিয়া সহজ করতে সাহায্য করে। যদি আরো কিছু জানতে চান বা অন্য কোনো প্রশ্ন থাকে, আমাকে জানান!
