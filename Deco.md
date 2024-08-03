## 21. Decorator Pattern

**Decorator Pattern** হল একটি স্ট্রাকচারাল ডিজাইন প্যাটার্ন যা একটি অবজেক্টে নতুন কার্যকলাপ যোগ করার একটি নমনীয় উপায় প্রদান করে, ক্লাসের পরিবর্তন না করে। এটি অবজেক্টগুলিকে নতুন কার্যকলাপ যোগ করার জন্য "র‌্যাপার" হিসেবে ব্যবহৃত হয়। 

### **What is the Decorator Pattern?**

**Decorator Pattern** একটি প্যাটার্ন যেখানে একটি অবজেক্টকে এমনভাবে র‌্যাপ করা হয় যাতে তার উপর অতিরিক্ত কার্যকলাপ যোগ করা যায়। এটি অবজেক্টের কম্পোজিশনের মাধ্যমে কার্যকলাপ যোগ করার অনুমতি দেয়, যা ইনহেরিট্যান্সের মাধ্যমে অর্জন করা কঠিন হতে পারে।

### **Components of Decorator Pattern**

1. **Component Interface**: এটি মূল অবজেক্টের ইন্টারফেস যা বেসিক কার্যকলাপ ডিফাইন করে।
2. **Concrete Component**: এটি ইন্টারফেসের একটি বাস্তবায়ন যা আসল অবজেক্ট হিসাবে কাজ করে।
3. **Decorator**: এটি ইন্টারফেসকে ইমপ্লিমেন্ট করে এবং একটি কম্পোনেন্টকে রেফারেন্স করে, অতিরিক্ত কার্যকলাপ যোগ করার জন্য।
4. **Concrete Decorators**: এগুলি ডেকোরেটর ক্লাসগুলির বাস্তবায়ন যা অতিরিক্ত কার্যকলাপ যোগ করে।

### **Example in PHP**

#### Step 1: Define the Component Interface

```php
interface Coffee {
    public function cost();
    public function description();
}
```

#### Step 2: Create a Concrete Component

```php
class SimpleCoffee implements Coffee {
    public function cost() {
        return 10;
    }

    public function description() {
        return "Simple Coffee";
    }
}
```

#### Step 3: Create the Decorator

```php
class CoffeeDecorator implements Coffee {
    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function cost() {
        return $this->coffee->cost();
    }

    public function description() {
        return $this->coffee->description();
    }
}
```

#### Step 4: Create Concrete Decorators

```php
class MilkDecorator extends CoffeeDecorator {
    public function cost() {
        return $this->coffee->cost() + 2;
    }

    public function description() {
        return $this->coffee->description() . ", Milk";
    }
}

class SugarDecorator extends CoffeeDecorator {
    public function cost() {
        return $this->coffee->cost() + 1;
    }

    public function description() {
        return $this->coffee->description() . ", Sugar";
    }
}
```

#### Step 5: Use the Decorators

```php
// Create a simple coffee
$coffee = new SimpleCoffee();
echo $coffee->description() . ": $" . $coffee->cost() . "\n";

// Add milk to the coffee
$coffee = new MilkDecorator($coffee);
echo $coffee->description() . ": $" . $coffee->cost() . "\n";

// Add sugar to the coffee
$coffee = new SugarDecorator($coffee);
echo $coffee->description() . ": $" . $coffee->cost() . "\n";
```

**Output**:
```
Simple Coffee: $10
Simple Coffee, Milk: $12
Simple Coffee, Milk, Sugar: $13
```

### **Advantages of Decorator Pattern**

1. **Flexibility**: অবজেক্টগুলিতে নতুন কার্যকলাপ যোগ করা যায় ক্লাস পরিবর্তন না করে।
2. **Reusability**: বিভিন্ন কনক্রিট ডেকোরেটরগুলি একত্রিত করা যায় যা কোডের পুনঃব্যবহারযোগ্যতা বাড়ায়।
3. **Single Responsibility Principle**: এটি ক্লাসগুলিকে একটি নির্দিষ্ট দায়িত্ব প্রদানের অনুমতি দেয় এবং অতিরিক্ত কার্যকলাপ আলাদাভাবে পরিচালনা করে।

### **When to Use the Decorator Pattern**

1. যখন নতুন কার্যকলাপ যোগ করার জন্য ক্লাসের পরিবর্তন না করা সম্ভব বা ইচ্ছাকৃত নয়।
2. যখন ক্লাসে একাধিক বৈশিষ্ট্য যোগ করার প্রয়োজন হয় এবং তা বিভিন্ন কনফিগারেশনে ব্যবহার করা হয়।
3. যখন অবজেক্টের কার্যকলাপ ক্রমানুসারে এবং স্বাধীনভাবে যোগ করতে হয়।

### **Conclusion**

**Decorator Pattern** একটি শক্তিশালী এবং নমনীয় প্যাটার্ন যা অবজেক্টগুলিতে নতুন কার্যকলাপ যোগ করতে ব্যবহৃত হয়, ক্লাসের পরিবর্তন না করে। এটি কোডের পুনঃব্যবহারযোগ্যতা, নমনীয়তা এবং সংরক্ষণযোগ্যতা উন্নত করতে সাহায্য করে।