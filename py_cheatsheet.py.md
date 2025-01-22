```python
# -*- coding: utf-8 -*-
"""
codes from https://www.youtube.com/playlist?list=PL98qAXLA6afuh50qD2MdAj3ofYjZR_Phn
"""

# ------- Object-oriented Programming (OOP) in Python (Easy to Understand Guide) #20 ------- #

print("\n\n\nObject-oriented Programming (OOP) in Python (Easy to Understand Guide) #20\n\n\n")

class Complex:
    def __init__(self, real, imag):
        self.real = real
        self.imag = imag
        
    def add(self, number):
        real = self.real + number.real
        imag = self.imag + number.imag
        result = Complex(real, imag)
        return result

n1 = Complex(5, 6)
n2 = Complex(-4, 2)
result = n1.add(n2)

print(dir(Complex))

print("real =", result.real)
print("imag =", result.imag)

# ------- #21: Everything is an Object in Python! ------- #

print("\n\n\n#21: Everything is an Object in Python! \n\n\n")

print(dir(list))

a = [1,2,3]
b = a
a.append(4)
print(id(a))
print(a)
print(id(b))
print(b)

a = [1,2,3]
b = a.copy()
a.append(4)
print(id(a))
print(a)
print(id(b))
print(b)

# ------- #22: Inheritance in Python (Derived/Child Classes) ------- #

print("\n\n\n#22: Inheritance in Python (Derived/Child Classes) \n\n\n")

class Animal:
    def eat(self):
        print("I can eat")
        
class Dog(Animal):
    def bark(self):
        print("I can bark")
    def eat(self):
        Animal.eat(self)
        super().eat()
        print("a dog can eat <-- overritten on top of the parent 'eat' method")

class Cat(Animal):
    def get_grumpy(self):
        print("I am getting grumpy.")

dog1 = Dog()
dog1.bark()
dog1.eat()

cat1 = Cat()
cat1.eat()

# ------- #29: Generators in Python: How to Use them & Why? ------- #

print("\n\n\n#29: Generators in Python: How to Use them & Why? \n\n\n")

def generate_odd():
    n = 1
    while True:
        yield n
        n += 2

odd_generator = generate_odd()

for num in range(10):
    print(next(odd_generator))
    
# ------- #31: Python Decorators ------- #

print("\n\n\n#31: Python Decorators \n\n\n")
    
def printer():
    print("Hello, World!")


def display_info(func):
    def inner():
        print("Executing",func.__name__,"function")
        func()
        print("Finished execution")
    return inner

printer()
print()
decorated_func = display_info(printer)
decorated_func()

print("\n\nalternative syntax using the @ to do the exact same thing\n\n")


def display_info(func):
    def inner():
        print("Executing",func.__name__,"function")
        func()
        print("Finished execution")
    return inner

@display_info
def printer():
    print("Hello, World!")

printer()

print()

def star(func):
    def inner(arg):
        print("*" * 30)
        func(arg)
        print("*" * 30)
    return inner

def percent(func):
    def inner(arg):
        print("%" * 30)
        func(arg)
        print("%" * 30)
    return inner

@star
@percent
def printer(msg):
    print(msg)

printer("Decorators are wonderful")

# ------- the diffrent uses of _, __, __*__, ... ------- #

# https://medium.com/python-features/naming-conventions-with-underscores-in-python-791251ac7097

'''
Underscore _: Used as a name for temporary variables.
Single Leading Underscore _var: Naming convention indicating name is meant for internal use. A hint for programmers and not enforced by programmers.
Double Leading Underscore __var: Triggers name mangling when used in class context. Enforced by the Python interpreter.
Single Trailing Underscore var_: Used by convention to avoid naming conflicts with Python keywords.
Double Trailing Underscore __var__: Indicates special methods defined by Python language (dunder methods).
'''


print("\n\n\nthe diffrent uses of _, __, __*__, ...\n\n\n")

class Person:
    def __init__(self):
        self.name = 'Sarah'
        self._age = 26
        self.__id = 30
        
p = Person()
print("Note that '__id' is not in dir(p) but instead '_Person__id':\n")
print(dir(p))

# ------- Use of vars() to include class atributes recursively ------- #

# Define a small class MyClass
class MyClass:
    def __init__(self):
        # Use vars(self) to access the class's dictionary of variables
        vars(self)['var1'] = 1

# Create an object of type MyClass()
my_obj = MyClass()
vars(my_obj)

```