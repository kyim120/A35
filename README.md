# C++ Object-Oriented Programming Concepts

## 7. Quick Quiz with Answers, Code & Explanation

### 🔁 Polymorphism

**Q1. What will be the output of this code using a base class pointer to call a derived class method?**

```cpp
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
}
```

**Answer:** `Derived class`

**Explanation:** Because `show()` in the base class is declared `virtual`, it enables runtime polymorphism. When called via a base class pointer, the overridden version in `Derived` is executed.

**Q2. Is function overloading an example of compile-time or runtime polymorphism?**

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

**Answer:** Compile-time polymorphism

**Explanation:** Function overloading is resolved at compile time based on parameter types.

**Q3. What happens if you remove the virtual keyword from the base class function?**

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
    b->show();
}
```

**Answer:** `Base`

**Explanation:** Without `virtual`, function calls are resolved at compile time using the pointer type. Here, `Base*` calls `Base::show()`.

**Q4. Can constructors be virtual in C++? Why or why not?**
**Answer:** No

**Explanation:** Virtual mechanism requires object instantiation, but constructors run before v-table is created. Hence, constructors can't be virtual.

**Q5. What does the `override` keyword do, and what happens if it's omitted?**
**Answer:** It tells the compiler that a method overrides a virtual base class method.

**Explanation:** If `override` is omitted, and there's a mismatch in the signature, the compiler won't catch it, causing potential bugs.

---

### 🧩 Aggregation

**Q6. If an object is passed into a constructor by pointer, does that indicate aggregation or composition?**

```cpp
class Engine {
public:
    void start() { cout << "Engine Start" << endl; }
};
class Car {
    Engine* engine;
public:
    Car(Engine* e) : engine(e) {}
};
```

**Answer:** Aggregation

**Explanation:** Aggregation means the lifetime of the part (Engine) is not managed by the container (Car). It exists independently.

**Q7. What happens to the aggregated object when the container is destroyed?**
**Answer:** It remains alive

**Explanation:** Since it's not owned by the container, it's not automatically deleted.

**Q8. Can multiple containers share the same aggregated object? Why?**
**Answer:** Yes

**Explanation:** Aggregated objects are independent and shared using pointers or references.

**Q9. Does aggregation imply memory management responsibility?**
**Answer:** No

**Explanation:** The object creating the aggregated class is responsible for deletion, not the container class.

**Q10. How would you identify aggregation by looking at code?**
**Answer:** Look for member pointers or references passed into constructors.

**Explanation:** Aggregation is indicated by external injection of objects, not internal instantiation.

---

### 🔗 Association

**Q11. Can an associated object exist without the class it's linked to?**
**Answer:** Yes

**Explanation:** Association is a relationship without ownership or lifecycle dependency.

**Q12. Is this a one-to-one, one-to-many, or many-to-many association?**

```cpp
class Student {
    Teacher* teacher;
};
```

**Answer:** Many-to-one

**Explanation:** Multiple students can point to one teacher.

**Q13. What kind of relationship exists if two classes just reference each other?**
**Answer:** Bidirectional Association

**Explanation:** Both classes maintain pointers or references to each other.

**Q14. How does association differ from inheritance in C++?**
**Answer:** Association is a usage relationship; inheritance is an "is-a" relationship.

**Explanation:** Association = communication. Inheritance = hierarchy.

**Q15. Can association exist without pointers or references?**
**Answer:** Yes

**Explanation:** Though uncommon, it's possible via values or global objects, but may lead to composition instead.

---

### 🧱 Composition

**Q16. What is the order of constructor and destructor calls in a composed class?**

```cpp
class Part {
public:
    Part() { cout << "Part\n"; }
};
class Whole {
    Part part;
public:
    Whole() { cout << "Whole\n"; }
};
int main() { Whole w; }
```

**Answer:** Constructor: Part → Whole; Destructor: Whole → Part

**Explanation:** Members are constructed before the containing class, and destroyed after it.

**Q17. Can you reuse a composed object across multiple container classes?**
**Answer:** No

**Explanation:** Composed objects are tightly bound to their container.

**Q18. How does the destruction of a container affect its composed members?**
**Answer:** They are automatically destroyed.

**Explanation:** Composition means ownership and destruction responsibility.

**Q19. Does composition imply strong or weak coupling?**
**Answer:** Strong coupling

**Explanation:** Composed parts are tightly integrated and managed internally.

**Q20. In what scenario would you prefer composition over inheritance?**
**Answer:** When you want flexibility or "has-a" relationship.

**Explanation:** Composition promotes modular and reusable design.

---

### 🧑‍🤝‍🧑 Friend Class & Function

**Q21. Can a friend function access private members of a class?**

```cpp
class Box {
    int value = 42;
    friend void show(Box);
};
void show(Box b) { cout << b.value; }
```

**Answer:** Yes

**Explanation:** `friend` allows non-member functions to access private data.

**Q22. Is friendship reciprocal between classes?**
**Answer:** No

**Explanation:** Friendship must be declared explicitly for each direction.

**Q23. Can a friend function be a member of another class?**
**Answer:** Yes

**Explanation:** As long as it's declared as a friend.

**Q24. What are the security implications of using friend functions?**
**Answer:** Breaks encapsulation

**Explanation:** Increases risk by exposing private implementation.

**Q25. Does declaring a friend class break encapsulation? Why or why not?**
**Answer:** Yes

**Explanation:** Friend classes can access all private/protected data.

---

### 🧬 Inheritance

**Q26. What is the output if both base and derived classes define the same function without `virtual`?**

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
    A* a = new B();
    a->show();
}
```

**Answer:** `A`

**Explanation:** Without `virtual`, the method called is resolved at compile-time using pointer type.

**Q27. How does constructor call order work in inheritance?**

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

**Answer:** `Base\nDerived`

**Explanation:** Base constructor is called before derived.

**Q28. What is the purpose of the `protected` access modifier in inheritance?**
**Answer:** Allows access in derived classes, but hides from outside code.

**Explanation:** Useful for members that need to be inherited but not exposed.

**Q29. What problem does virtual inheritance solve?**
**Answer:** Diamond problem

**Explanation:** Ensures only one instance of base class is shared in multiple inheritance.

**Q30. Can a derived class inherit private members from a base class? How?**
**Answer:** Yes, but not accessible directly

**Explanation:** They can be used indirectly via public/protected base methods.

---

# Thank You!
