## 37. Observer Pattern
**Observer Pattern** একটি ডিজাইন প্যাটার্ন যা একটি অবজেক্ট (subject) পরিবর্তিত হলে তার সাথে সংযুক্ত থাকা অন্যান্য অবজেক্টগুলিকে (observers) অবহিত করতে ব্যবহৃত হয়। এটি একটি এক-থেকে-মাল্টিপল রিলেশনশিপ তৈরি করে যেখানে একটি সিঙ্গেল অবজেক্টের পরিবর্তন একাধিক অবজেক্টে প্রভাব ফেলে।

### **Core Concepts**

1. **Subject (Observable):** এটি এমন একটি অবজেক্ট যা পরিবর্তিত হলে তার অবজেক্টগুলিকে নোটিফাই করে। এটি অবজার্ভারদের তালিকা রাখে এবং তাদের নোটিফাই করে যখন তার স্টেট পরিবর্তিত হয়।

2. **Observer:** এটি এমন একটি অবজেক্ট যা সিস্টেমের পরিবর্তন সম্পর্কে জানাতে আগ্রহী থাকে। যখন সাবজেক্ট পরিবর্তিত হয়, অবজার্ভারকে নোটিফাই করা হয় এবং এটি সেবার পরিবর্তনগুলি গ্রহণ করে।

3. **ConcreteSubject:** এটি একটি বিশেষ ধরনের সাবজেক্ট যা সঠিক স্টেট ধারণ করে এবং পরিবর্তনগুলি অন্যান্য অবজেক্টে নোটিফাই করে।

4. **ConcreteObserver:** এটি একটি বিশেষ ধরনের অবজেক্ট যা সঠিকভাবে সিস্টেমের পরিবর্তনগুলি গ্রহণ করে এবং প্রক্রিয়া করে।

### **Example**

#### **Scenario:** একটি নিউজ অ্যাপ্লিকেশন যা বিভিন্ন সাবস্ক্রাইবারদের (ওবজার্ভার) সাথে নিউজ আপডেট পাঠায়।

```php
// Observer Interface
interface Observer {
    public function update($news);
}

// Subject Interface
interface Subject {
    public function attach(Observer $observer);
    public function detach(Observer $observer);
    public function notify();
}

// Concrete Subject
class NewsAgency implements Subject {
    private $observers = [];
    private $news;

    public function attach(Observer $observer) {
        $this->observers[] = $observer;
    }

    public function detach(Observer $observer) {
        $this->observers = array_filter($this->observers, function ($obs) use ($observer) {
            return $obs !== $observer;
        });
    }

    public function setNews($news) {
        $this->news = $news;
        $this->notify();
    }

    public function notify() {
        foreach ($this->observers as $observer) {
            $observer->update($this->news);
        }
    }
}

// Concrete Observer
class NewsChannel implements Observer {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function update($news) {
        echo $this->name . " received news: " . $news . "\n";
    }
}

// Usage
$agency = new NewsAgency();

$channel1 = new NewsChannel("Channel 1");
$channel2 = new NewsChannel("Channel 2");

$agency->attach($channel1);
$agency->attach($channel2);

$agency->setNews("Breaking News: Observer Pattern Explained!");

// Output:
// Channel 1 received news: Breaking News: Observer Pattern Explained!
// Channel 2 received news: Breaking News: Observer Pattern Explained!
```

### **Benefits of Observer Pattern**

1. **Loose Coupling:** অবজেক্টগুলি একে অপরের সাথে কম্পনেন্টিং করে, ফলে তাদের মধ্যে লুজ কপ্লিং হয়। এটি একটি অবজেক্ট পরিবর্তিত হলে অন্যান্য অবজেক্টগুলিতে পরিবর্তন ঘটায়।

2. **Dynamic Relationships:** সাবজেক্ট এবং অবজেক্টগুলির মধ্যে সম্পর্কগুলি রানটাইমে পরিবর্তনযোগ্য হয়। অবজেক্টগুলির যোগ ও বাদ দেয়া সহজ হয়।

3. **Multiple Observers:** একাধিক অবজেক্ট একই সময়ে একটি সিঙ্গেল অবজেক্টের পরিবর্তনগুলির জন্য সাবস্ক্রাইব করতে পারে।

4. **Separation of Concerns:** অবজেক্টের অবস্থা পরিবর্তনের সাথে যুক্ত লজিক অন্য অবজেক্টে বিচ্ছিন্ন থাকে, যা কোডের পরিষ্কারতা এবং রক্ষণাবেক্ষণ সহজ করে।

### **Summary**

**Observer Pattern** একটি ডিজাইন প্যাটার্ন যা একটি অবজেক্টের (সাবজেক্ট) পরিবর্তন হলে অন্যান্য অবজেক্টগুলিকে (ওবজার্ভার) নোটিফাই করে। এটি মূলত **Subject** (অবজেক্ট যা পরিবর্তিত হয়), **Observer** (অবজেক্ট যা নোটিফাই হয়), **ConcreteSubject** এবং **ConcreteObserver** সহ বিভিন্ন উপাদান দ্বারা গঠিত। এই প্যাটার্নটি লুজ কপ্লিং, ডায়নামিক রিলেশনশিপ, এবং একাধিক অবজেক্টের জন্য একটি কার্যকরী সমাধান প্রদান করে।
