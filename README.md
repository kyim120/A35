# C++ Object-Oriented Programming Concepts

This document explains essential C++ OOP (Object-Oriented Programming) topics in detail, including Polymorphism, Aggregation, Association, Composition, Friend Class & Function, and Inheritance.

---

### 🔁 **Polymorphism**

---

### ✅ **Q1. What will be the output of this code using a base class pointer to call a derived class method?**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void show() {
        cout << "Base class" << endl;
    }
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived class" << endl;
    }
};

int main() {
    Base* b = new Derived();
    b->show();
    return 0;
}
```

**🧠 Answer:** `Derived class`

**📘 Explanation:**
Since the `show()` function in `Base` is marked `virtual`, this enables **runtime polymorphism**. The actual method invoked depends on the object the pointer points to — here, a `Derived` object. So `Derived::show()` is executed even though the pointer is of type `Base*`.

---

### ✅ **Q2. Is function overloading an example of compile-time or runtime polymorphism?**

```cpp
class Example {
public:
    void display(int x) {
        cout << "Integer: " << x << endl;
    }

    void display(double y) {
        cout << "Double: " << y << endl;
    }
};
```

**🧠 Answer:** Compile-time polymorphism

**📘 Explanation:**
Function overloading is resolved by the compiler **at compile time** based on the function signature (i.e., number and types of parameters). This is a classic example of **static binding** or **compile-time polymorphism**.

---

### ✅ **Q3. What happens if you remove the `virtual` keyword in a polymorphic base class function?**

```cpp
class Base {
public:
    void show() {
        cout << "Base" << endl;
    }
};

class Derived : public Base {
public:
    void show() {
        cout << "Derived" << endl;
    }
};

int main() {
    Base* b = new Derived();
    b->show();  // What gets called?
}
```

**🧠 Answer:** `Base`

**📘 Explanation:**
If `virtual` is not used, the function call is **statically bound** to the type of the pointer — not the actual object. So even though `b` points to a `Derived`, the `Base::show()` function is invoked. This is **not polymorphism**.

---

### ✅ **Q4. Can constructors be virtual in C++? Why or why not?**

**🧠 Answer:** No, constructors cannot be virtual in C++.

**📘 Explanation:**
Virtual functions rely on the **v-table**, which is initialized during object construction. But constructors are executed **before** the v-table is fully set up, so there's no mechanism to make them virtual.
However, **destructors** should be virtual in polymorphic base classes to avoid memory leaks.

---

### ✅ **Q5. What does the `override` keyword do, and what happens if it's omitted?**

```cpp
class Base {
public:
    virtual void speak() {
        cout << "Base speaking" << endl;
    }
};

class Derived : public Base {
public:
    void speak() override {
        cout << "Derived speaking" << endl;
    }
};
```

**🧠 Answer:**
The `override` keyword ensures the method is actually overriding a base class virtual function.

**📘 Explanation:**

* If `override` is used and the function **does not match** a virtual function in the base, the compiler gives an **error**.
* If `override` is **omitted**, and there’s a typo in the method name or mismatch in signature, it creates a **new function** (not an override), leading to subtle bugs.

---

### 🧩 **Aggregation**

---

### ✅ **Q1. If an object is passed into a constructor by pointer, does that indicate aggregation or composition?**

```cpp
class Engine {
public:
    void start() {
        cout << "Engine started" << endl;
    }
};

class Car {
    Engine* engine;  // Aggregated (not owned)
public:
    Car(Engine* eng) : engine(eng) {}
    void drive() {
        engine->start();
        cout << "Car is moving" << endl;
    }
};
```

**🧠 Answer:** Aggregation

**📘 Explanation:**
Aggregation is a **“has-a”** relationship where the container (Car) holds a reference or pointer to another object (Engine) **without owning it**. Since `Engine` is passed into `Car` externally and is not created inside `Car`, this is aggregation — not composition.

---

### ✅ **Q2. What happens to the aggregated object when the container object is destroyed?**

**🧠 Answer:** The aggregated object **is not destroyed automatically**.

**📘 Explanation:**
In aggregation, the lifetime of the part (`Engine`) is **independent** of the whole (`Car`). When `Car` is destroyed, `Engine` still exists unless manually deleted elsewhere. This differs from composition, where destruction of the container destroys the composed objects too.

---

### ✅ **Q3. Can multiple container objects share the same aggregated object? Why?**

```cpp
int main() {
    Engine* e = new Engine();
    Car car1(e);
    Car car2(e);  // Shared Engine

    car1.drive();
    car2.drive();
}
```

**🧠 Answer:** Yes, multiple objects can share an aggregated object.

**📘 Explanation:**
Since aggregation uses **external pointers or references**, multiple containers (e.g., multiple `Car`s) can safely use the same `Engine`. This shared ownership is a key difference from composition, which implies exclusive ownership.

---

### ✅ **Q4. Does aggregation imply memory management responsibility?**

**🧠 Answer:** No, it does not.

**📘 Explanation:**
In aggregation, the container class (e.g., `Car`) **does not own or manage** the lifecycle of the aggregated object (`Engine`). Whoever creates the object (like `main()` or another controller class) is responsible for deleting it. This is why it's often paired with smart pointers to avoid memory leaks.

---

### ✅ **Q5. How would you identify aggregation by just looking at the code?**

**🧠 Answer:** Look for **pointers or references** passed into the class, and the object **not being instantiated** inside the class.

```cpp
class Department;
class Company {
    Department* dept;  // Not owned
public:
    Company(Department* d) : dept(d) {}
};
```

**📘 Explanation:**

* If the object is passed by pointer or reference → likely aggregation
* If the class **does not use `new`** to create the object → it doesn't own it
* If the object can exist independently → it’s aggregation

In contrast, composition involves **creating the object inside the class**, often by value or using `new`, and destroying it with the container.

---

Great! Let's continue with detailed answers, code examples, and explanations for each group you listed.

---

## 🔗 **Association**

---

### ✅ **Q1. Can an associated object exist without the class it's linked to?**

**🧠 Answer:** Yes

**📘 Explanation:**
Association represents a **usage relationship**. It does not imply ownership. For example, a `Doctor` and a `Patient` can exist independently.

```cpp
class Patient {
public:
    void getInfo() { cout << "Patient info\n"; }
};

class Doctor {
    Patient* patient;  // Association
public:
    Doctor(Patient* p) : patient(p) {}
    void treat() { patient->getInfo(); }
};
```

---

### ✅ **Q2. Is this a one-to-one, one-to-many, or many-to-many association?**

**🧠 Answer:** It depends on the design.

**📘 Explanation:**

* **One-to-One**: A teacher mentors one student.
* **One-to-Many**: A teacher teaches multiple students.
* **Many-to-Many**: Students attend multiple courses, and courses have multiple students.

```cpp
class Student;
class Course {
    vector<Student*> students;
};
```

---

### ✅ **Q3. What kind of relationship exists if two classes just reference each other?**

**🧠 Answer:** Bidirectional Association

**📘 Explanation:**
If two classes refer to each other (usually via pointers), it's a **bidirectional association**. Common in team systems, like:

```cpp
class Manager;

class Employee {
    Manager* mgr;
};

class Manager {
    vector<Employee*> team;
};
```

---

### ✅ **Q4. How does association differ from inheritance in C++?**

**🧠 Answer:**

* Association is a **has-a** or **uses-a** relationship.
* Inheritance is an **is-a** relationship.

**📘 Explanation:**

```cpp
// Association
class Driver {
    Car* car;
};

// Inheritance
class ElectricCar : public Car {
};
```

Inheritance shares behavior. Association connects classes without hierarchical dependency.

---

### ✅ **Q5. Can association exist without pointers or references?**

**🧠 Answer:** Yes, but rare.

**📘 Explanation:**
Association usually uses references or pointers. But you can associate by value — though this starts to resemble **composition** if the object is created/owned.

```cpp
class Office {
    Printer printer; // Composition-like
};
```

To be safe, use pointers/references for association.

---

## 🧱 **Composition**

---

### ✅ **Q1. What is the order of constructor and destructor calls in a composed class?**

```cpp
class Part {
public:
    Part() { cout << "Part\n"; }
    ~Part() { cout << "~Part\n"; }
};

class Whole {
    Part p;
public:
    Whole() { cout << "Whole\n"; }
    ~Whole() { cout << "~Whole\n"; }
};

int main() {
    Whole w;
}
```

**🧠 Answer:**
Constructor: `Part` → `Whole`
Destructor: `~Whole` → `~Part`

**📘 Explanation:**
Composed parts are constructed **before** and destroyed **after** the container.

---

### ✅ **Q2. Can you reuse a composed object across multiple container classes?**

**🧠 Answer:** No

**📘 Explanation:**
Composition implies **exclusive ownership**. The object is created and destroyed by the container. You cannot share it like you would in aggregation.

---

### ✅ **Q3. How does the destruction of a container affect its composed members?**

**🧠 Answer:**
They are destroyed **automatically** when the container is destroyed.

**📘 Explanation:**
This ensures safe cleanup and reflects strong ownership.

---

### ✅ **Q4. Does composition imply strong or weak coupling?**

**🧠 Answer:** Strong coupling

**📘 Explanation:**
Composed parts cannot be removed or changed independently. This means the container relies heavily on its components.

---

### ✅ **Q5. In what scenario would you prefer composition over inheritance?**

**🧠 Answer:**
When you want **flexibility** or need a **has-a** relationship.

**📘 Explanation:**
Composition allows better modular design. You can switch out components without changing class hierarchy.

---

## 🧑‍🤝‍🧑 **Friend Class & Function**

---

### ✅ **Q1. Can a friend function access private data members of a class?**

```cpp
class Secret {
    int code = 123;
    friend void reveal(Secret);
};

void reveal(Secret s) {
    cout << s.code;
}
```

**🧠 Answer:** Yes

**📘 Explanation:**
Friend functions are **not members** but can access **private** and **protected** members.

---

### ✅ **Q2. Is friendship reciprocal between classes?**

**🧠 Answer:** No

**📘 Explanation:**
Friendship must be **explicitly declared** both ways. If `A` is a friend of `B`, `B` is not a friend of `A` unless declared.

---

### ✅ **Q3. Can a friend function be a member of another class?**

```cpp
class A;
class B {
public:
    void show(A&); // can be friend
};

class A {
    int value = 5;
    friend void B::show(A&);
};
```

**🧠 Answer:** Yes

**📘 Explanation:**
A member function of another class can be declared as a **friend**, giving it access to private members.

---

### ✅ **Q4. What are the security implications of using friend functions?**

**🧠 Answer:** It breaks **encapsulation**.

**📘 Explanation:**
Friend functions bypass access restrictions. Overuse can expose internal details and make code harder to maintain.

---

### ✅ **Q5. Does declaring a friend class break encapsulation? Why or why not?**

**🧠 Answer:** Yes

**📘 Explanation:**
The friend class gets full access to all private/protected members. This weakens the principle of **data hiding**.

---

## 🧬 **Inheritance**

---

### ✅ **Q1. What is the output if both base and derived classes define the same function without `virtual`?**

```cpp
class A {
public:
    void show() { cout << "A"; }
};

class B : public A {
public:
    void show() { cout << "B"; }
};

int main() {
    A* ptr = new B();
    ptr->show();
}
```

**🧠 Answer:** `A`

**📘 Explanation:**
Without `virtual`, the method is resolved **at compile-time** based on pointer type.

---

### ✅ **Q2. How does the constructor call order work in inheritance?**

```cpp
class Base {
public:
    Base() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    Derived() { cout << "Derived\n"; }
};
```

**🧠 Answer:**
`Base` → `Derived`

**📘 Explanation:**
Base class constructor runs **before** derived class constructor.

---

### ✅ **Q3. What is the purpose of the `protected` access modifier in inheritance?**

**🧠 Answer:**
To allow derived classes access to members that are not public.

**📘 Explanation:**
`protected` members are **hidden from external code** but available to child classes. Good for controlled inheritance.

---

### ✅ **Q4. What problem does virtual inheritance solve?**

**🧠 Answer:** The **diamond problem**

**📘 Explanation:**
If two parent classes inherit the same grandparent, virtual inheritance ensures only **one instance** of the grandparent is shared.

```cpp
class A { };
class B : virtual public A { };
class C : virtual public A { };
class D : public B, public C { };
```

---

### ✅ **Q5. Can a derived class inherit private members from a base class? How?**

**🧠 Answer:**
Yes, but they are **not accessible directly**.

**📘 Explanation:**
Private members are inherited but can't be accessed by name in the derived class. You must use base class public/protected functions to manipulate them.

---
## Any Questions Fee free to DM.
