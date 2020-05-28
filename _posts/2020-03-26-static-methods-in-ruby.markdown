---
layout: default
title:  "Static methods in ruby"
description: Explaining why static methods are not static methods, but rather singleton methods using example.
date:   2020-03-26 00:00:00 -0000
categories: main
---
# Static methods in ruby, explained:

Consider the following lines:

   ```ruby
   class AClass
      def method1
       puts 'Method1 is present'
      end
      def self.method2
       puts 'Method2 is present'
      end
   end

   AClass.method1 # undefined method `method1'
   AClass.method2 # prints 'Method2 is present'

   obj = AClass.new
   
   def obj.method1
      puts 'Method1 is present, modified by obj'
   end

   def obj.method3
      puts 'Method3 is present'
   end

   obj.method3 # prints 'Method3 is present'
   
   obj.methods
   # =>  [:method3, :method1,....]
   
   AClass.methods
   # => [:method2, ...]
   
   AClass.new.methods
   # => [:method1, ...]

   AClass.singleton_methods
   # => [:method2] 
   ```

From the above example, few learnings:
1. It's clear that an object can override a class method implementation

2. ```ruby
   class SomeClass
      def self.some_method do 
      end
   end
   ``` 
   is similar to:
   ```ruby
   obj = SomeClass.new
   def obj.some_mehtod do 
   end
   ```
   so that we can call `obj.some_method`. in the above example we can call `SomeClass.some_method`.
   
   Thus eventhough  `SomeClass.some_method` looks like calling a static method, It's not. What is actually happening is the singleton object of the class is called. *Every class has a singleton object in ruby.*