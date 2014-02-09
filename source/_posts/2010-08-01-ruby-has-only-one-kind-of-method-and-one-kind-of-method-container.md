---
layout: post
title: Ruby has only one kind of method and one kind of method container
date: 2010-08-01
tags:
- Ruby
- programming
- metaprogramming
comments: true
---
While learning Ruby, I found the talk of classes, modules, eigenclasses (also
known as singleton classes or metaclasses), instance methods, class methods and
singleton methods (also known as eigenmethods) pretty confusing. Fortunately,
when you put everything together<sup>[1]</sup>, it turns out to be very simple: Ruby has
only one kind of methods, which are instance methods of modules. The methods
that are accessible to an object, are the instance methods of its eigenclass
and the superclasses of the eigenclass.

If you’re thinking "no, Ruby also has class methods or singleton methods": as
Ola Bini shows in [his article on singleton classes](http://ola-bini.blogspot.com/2006/09/ruby-singleton-class.html):
defining a class method or singleton method is identical to defining an instance
method on its eigenclass. Now if you define the method using either syntax, then
where does this method actually live? Both:
```ruby
C1.singleton_methods

# and

(class << C1; self; end).instance_methods
```
will report the method, but of course only one instance of this method exists.
The [Rubinius](http://rubini.us/) implementation of Ruby reflects this:
```ruby
def singleton_methods(all=true)
  mt = Rubinius.object_metaclass(self).method_table
  methods = (all ? mt.names : mt.public_names + mt.protected_names)

  Rubinius.convert_to_names methods
end
```
When you ask for the singleton methods of a class, you get the methods (the
method_table) of its metaclass.

The consequence of this is that it gets much easier to reason about the
behavior of [extend](http://ruby-doc.org/core/classes/Object.html#M000335) and
[include](http://ruby-doc.org/core/classes/Module.html#M001638): extending a
module is equivalent to adding its instance methods to the instance methods of
the eigenclass of the extending module. That you can access them via
`ExtendingModule.module_instance_method` is merely syntactical sugar.

Including a module is equivalent to adding the module’s instance methods to the
instance methods of the class. The instance methods of a class are accessible
to instances of the class, which is how an instance can have access to the
included module’s instance methods.

Examples:
```ruby
module M1
  def self.module_singleton_method
    "#{self}.module_singleton_method"
  end

  def module_instance_method
    "#{self}.module_instance_method"
  end

  class << self
    def module_eigenclass_instance_method
      "#{self}.module_eigenclass_instance_method"
    end
    if !instance_methods.include? module_singleton_method
      raise StandardError, "This wont happen."
    end
  end
end

module M2; extend M1; end

module M3; include M1; end

module M4; extend M3; end

# Everything that raises an Error is commented

# You cant subclass a module:
# module M5 < M1; end

# You cant instantiate modules: Module doesnt have a new method.
# M1.new

puts "Accessing methods through an instance of Module"
puts M1.module_singleton_method
# puts M1.module_instance_method
puts M1.module_eigenclass_instance_method
# puts M2.module_singleton_method
puts M2.module_instance_method
# puts M2.module_eigenclass_instance_method
# puts M3.module_singleton_method
# puts M3.module_instance_method
# puts M3.module_eigenclass_instance_method
# puts M4.module_singleton_method
puts M4.module_instance_method
# puts M4.module_eigenclass_instance_method
```

What can we conclude from this?

* You cannot access a module’s instance methods, except by extending the
  module. When you extend a module, its instance methods are added as
  instance methods to the eigenclass of the extending module. In the
  example, M1’s instance methods become instance methods of M2’s
  eigenclass.
* Including a module into another module makes the instance methods of the
  included module available as instance methods in the including module.
  This may seem pointless, because you still can’t access them, but
  subsequently extending the including module also makes the included
  module’s methods available to the extending module. In the example, M1’s
  instance methods were added to M3’s and through the extend they became to
  M4.
* Defining a singleton method on a module has exactly the same result as
  defining an instance method on a module’s eigenclass. Neither extending
  nor including the module will make the instance methods of its eigenclass
  available to the extending or including module.
* You can only inherit from a class, not from a module.

For classes, it’s pretty much the same:
```ruby
class C1
  def self.class_singleton_method
    "#{self}.class_singleton_method"
  end

  def class_instance_method
    "#{self}.class_instance_method"
  end

  class << self
    def class_eigenclass_instance_method
      "#{self}.module_eigenclass_instance_method"
    end
    if !instance_methods.include? class_singleton_method
      raise StandardError
    end
  end
end

class C2 < C1; end

class C3; extend M1; end

class C4; include M1; end

# class C5; extend C1; end
# class C6; include C1; end

puts "Accessing methods through an instance of Class"
puts C1.class_singleton_method
# puts C1.class_instance_method
puts C1.class_eigenclass_instance_method
puts C2.class_singleton_method
# puts C2.class_instance_method
puts C2.class_eigenclass_instance_method
# puts C3.module_singleton_method
puts C3.module_instance_method
# puts C3.module_eigenclass_instance_method
# puts C4.module_singleton_method
# puts C4.module_instance_method
# puts C4.module_eigenclass_instance_method

puts "Accessing methods through a class instance"
# puts C1.new.class_singleton_method
puts C1.new.class_instance_method
# puts C1.new.class_eigenclass_instance_method
# puts C2.new.class_singleton_method
puts C2.new.class_instance_method
# puts C2.new.class_eigenclass_instance_method
# puts C3.new.module_singleton_method
# puts C3.new.module_instance_method
# puts C3.new.module_eigenclass_instance_method
# puts C4.new.module_singleton_method
puts C4.new.module_instance_method
# puts C4.new.module_eigenclass_instance_method
```

From this we can conclude:

* Instance methods of a class are accessible to each instance of the class.
  Each instance has it’s own unique eigenclass, but when a method isn’t
  found there, the next stop is the superclass of the eigenclass, which is
  the object representing the same class we usually consider the class of
  the instance.
* You can only extend or include modules, not classes.
* When a class extends a module, the module’s instance methods are added to
  the instance methods of the class’s eigenclass. Since classes are
  modules, this is exactly as expected.
* Including a module into a class makes the module’s instance methods
  available to all instances of the class.

--------------------------------

<sup>[1]</sup> With a little help from the excellent:
[Metaprogramming Ruby](http://pragprog.com/titles/ppmetr/metaprogramming-ruby) by Paolo Perrotta
