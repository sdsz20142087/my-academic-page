---
title: Design Pattern --- Creational Pattern
date: 2022-04-24T03:49:05.326Z
summary: "***Singleton Pattern, Simple Factory Pattern, Factory Method Pattern,
  Abstract Factory Pattern, Builder Pattern, ProtoType Pattern***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Singleton Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Ensure a class has only one instance, and provide a global point of access to it.

class Singleton{
public:
    static Singleton* getInstance(string name) {
        if (_instance == nullptr) {
            _mtx.lock();
            cout << name << " ";
            if (_instance == nullptr) {
                _instance = new Singleton();
            }
            cout << _instance << endl;
            _mtx.unlock();
        }
        return _instance;
    }
private:
    Singleton() {
        this_thread::sleep_for(chrono::milliseconds(1));
    }
    Singleton(const Singleton&) = delete;
    Singleton(const Singleton&&) = delete;
    static Singleton* _instance;
    static mutex _mtx;
};
Singleton* Singleton::_instance = nullptr;
mutex Singleton::_mtx;

// Singleton* Singleton::getInstance(string name) {
//  if (_instance == nullptr) {
//      _mtx.lock();
//      cout << name << " ";
//      if (_instance == nullptr) {
//          _instance = new Singleton();
//      }
//      cout << _instance << endl;
//      _mtx.unlock();
//  }
//  return _instance;
// }

class Singleton2{
public:
    static Singleton2* getInstance(string name) {
        static Singleton2* _instance = new Singleton2();
        return _instance;
    }

private:
    Singleton2() {
        this_thread::sleep_for(chrono::milliseconds(1));
    }
    Singleton2(const Singleton2&) = delete;
    Singleton2(const Singleton2&&) = delete;
};

void test2(string name) {
    Singleton2* s = Singleton2::getInstance(name);
    cout << s << endl;
    return ;
}

void test(string name) {
    Singleton* s = Singleton::getInstance(name);
    this_thread::sleep_for(chrono::milliseconds(10));
    test2(name);
    return ;
}

int main() {
    thread t1(test,"t1"), t2(test,"t2"), t3(test,"t3"), t4(test,"t4"), t5(test,"t5");
    t1.join();t2.join();t3.join();t4.join();t5.join();
    return 0;
}

```

## 2. Simple Factory Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;

class Product{
public:
    virtual~ Product() {}
    virtual void ProductName() = 0;
};

class Product1 : public Product{
public:
    void ProductName() override{
        cout << "Product1" << endl;
    }
};

class Product2 : public Product{
public:
    void ProductName() override{
        cout << "Product2" << endl;
    }
};

class Product3 : public Product{
public:
    void ProductName() override{
        cout << "Product3" << endl;
    }
};

class SimpleFactory{
public:
    Product* getProduct(int number) {
        switch(number) {
            case 1 : return new Product1();
            case 2 : return new Product2();
            case 3 : return new Product3();
        }
        return nullptr;
    }
};
void output(Product* product) {
    product->ProductName();
    delete(product);
    return ;
}

int main() {
    SimpleFactory* factory = new SimpleFactory();
    output(factory->getProduct(1));
    output(factory->getProduct(2));
    output(factory->getProduct(3));
    return 0;
}

```

## 3. Factory Method Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
class Product{
public:
    virtual~ Product() {}
    virtual void ProductName() = 0;
};

class Product1 : public Product{
public:
    void ProductName() override{
        cout << "Product1" << endl;
    }
};

class Product2 : public Product{
public:
    void ProductName() override{
        cout << "Product2" << endl;
    }
};

class Product3 : public Product{
public:
    void ProductName() override{
        cout << "Product3" << endl;
    }
};

class Factory{
public:
    virtual Product* getProduct() = 0;
};

class concreteFactory1 : public Factory{
public:
    Product* getProduct() {
        return new Product1();
    }
};

class concreteFactory2 : public Factory{
public:
    Product* getProduct() {
        return new Product2();
    }
};

class concreteFactory3 : public Factory{
public:
    Product* getProduct() {
        return new Product3();
    }
};

void output(Product* product) {
    product->ProductName();
    delete(product);
    return ;
}

int main() {
    Factory* factory = new concreteFactory1();
    output(factory->getProduct());
    factory = new concreteFactory2();
    output(factory->getProduct());
    factory = new concreteFactory3();
    output(factory->getProduct());
    return 0;
}

```

## 4. Abstract Factory Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
class AbstractProduct1{
public:
    AbstractProduct1() {cout << "build class AbstractProduct1" << endl;}
    virtual~ AbstractProduct1() {}
};

class AbstractProduct2{
public:
    AbstractProduct2() {cout << "build class AbstractProduct2" << endl;}
    virtual~ AbstractProduct2() {}
};

class AbstractFactory{
public:
    virtual void buildProduct1() = 0;
    virtual void buildProduct2() = 0;
};

class ConcreteFactoryA : public AbstractFactory{
public:
    void buildProduct1() override {
        cout << "Factory A" << endl;
        AbstractProduct1 product1;
        return ;
    }
    void buildProduct2() override {
        cout << "Factory A" << endl;
        AbstractProduct2 product2;
        return ;
    }
};

class ConcreteFactoryB : public AbstractFactory{
public:
    void buildProduct1() override {
        cout << "Factory B" << endl;
        AbstractProduct1 product1;
        return ;
    }
    void buildProduct2() override {
        cout << "Factory B" << endl;
        AbstractProduct2 product2;
        return ;
    }
};

int main() {
    AbstractFactory* Factory = new ConcreteFactoryA();
    Factory->buildProduct1();
    Factory->buildProduct2();
    delete(Factory);
    Factory = new ConcreteFactoryB();
    Factory->buildProduct1();
    Factory->buildProduct2();
    delete(Factory);
    return 0;
}

```

## 5. Builder Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Separate the construction of a complex object form its representation so that the same construction process can create different representations.
class House{
public:
    void set_color(int color) {
        _color = color;
    }
    void set_height(int height) {
        _height = height;
    }
    void set_decoration(int decoration) {
        _decoration = decoration;
    }
    void output() {
        cout << _color << " " << _height << " " << _decoration << endl;
    }
private:
    int _color, _height, _decoration;
};

class Builder{
public:
    virtual void buildColor() = 0;
    virtual void buildHeight() = 0;
    virtual void buildDecoration() = 0;
    virtual House* getHouse() = 0;
};

class concreteBuilder1 : public Builder{
public:
    ~concreteBuilder1() {delete _house;}
    void buildColor() override {
        _house->set_color(1);
        cout << "1. buildColor" << endl;
    }
    void buildHeight() override {
        _house->set_height(11);
        cout << "1. buildHeight" << endl;
    }   
    void buildDecoration() override {
        _house->set_decoration(111);
        cout << "1. buildDecoration" << endl;
    }
    House* getHouse() {
        _house = new House();
        return _house;
    }
private:
    House* _house;
};

class concreteBuilder2 : public Builder{
public:
    ~concreteBuilder2() {delete _house;}
    void buildColor() override {
        _house->set_color(2);
        cout << "2. buildColor" << endl;
    }
    void buildHeight() override {
        _house->set_height(22);
        cout << "2. buildHeight" << endl;
    }   
    void buildDecoration() override {
        _house->set_decoration(222);
        cout << "2. buildDecoration" << endl;
    }
    House* getHouse() {
        _house = new House();
        return _house;
    }
private:
    House* _house;
};

class Director{
public:
    Director(Builder* builder = nullptr) : _builder(builder) {}
    ~Director() {delete _builder;}
    House* direct() {
        if (_builder == nullptr) return nullptr;
        House* house = _builder->getHouse();
        _builder->buildColor();
        _builder->buildHeight();
        _builder->buildDecoration();
        return house;
    }
private:
    Builder* _builder;
};
int main() {
    Director director1(new concreteBuilder1()), director2(new concreteBuilder2());
    House* house = director1.direct();
    house->output();
    house = director2.direct();
    house->output();
    return 0;
}

```

## 6. ProtoType Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//　Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
class House{
public:
    void set_color(int color) {
        _color = color;
    }
    void set_height(int height) {
        _height = height;
    }
    void set_decoration(int decoration) {
        _decoration = decoration;
    }
    void output() {
        cout << _color << " " << _height << " " << _decoration << endl;
    }
private:
    int _color, _height, _decoration;
};

class Builder{
public:
    virtual Builder* copy() = 0;
    virtual void buildColor() = 0;
    virtual void buildHeight() = 0;
    virtual void buildDecoration() = 0;
    virtual House* getHouse() = 0;
};

class concreteBuilder1 : public Builder{
public:
    ~concreteBuilder1() {delete _house;}
    Builder* copy() override {
        return new concreteBuilder1(*this);
    }

    void buildColor() override {
        _house->set_color(1);
        cout << "1. buildColor" << endl;
    }
    void buildHeight() override {
        _house->set_height(11);
        cout << "1. buildHeight" << endl;
    }   
    void buildDecoration() override {
        _house->set_decoration(111);
        cout << "1. buildDecoration" << endl;
    }
    House* getHouse() {
        _house = new House();
        return _house;
    }
private:
    House* _house;
};

class concreteBuilder2 : public Builder{
public:
    ~concreteBuilder2() {delete _house;}
    Builder* copy() override {
        return new concreteBuilder2(*this);
    }

    void buildColor() override {
        _house->set_color(2);
        cout << "2. buildColor" << endl;
    }
    void buildHeight() override {
        _house->set_height(22);
        cout << "2. buildHeight" << endl;
    }   
    void buildDecoration() override {
        _house->set_decoration(222);
        cout << "2. buildDecoration" << endl;
    }
    House* getHouse() {
        _house = new House();
        return _house;
    }
private:
    House* _house;
};

class Director{
public:
    Director(Builder* builder = nullptr) : _builder(builder) {}
    ~Director() {delete _builder;}
    House* direct() {
        if (_builder == nullptr) return nullptr;
        House* house = _builder->getHouse();
        _builder->buildColor();
        _builder->buildHeight();
        _builder->buildDecoration();
        return house;
    }
private:
    Builder* _builder;
};
int main() {
    // Director director1(new concreteBuilder1()), director2(new concreteBuilder2());
    // House* house = director1.direct();
    // house->output();
    // house = director2.direct();
    // house->output();
    Builder* builder = new concreteBuilder1();
    //Builder* builderCopy = new Builder(builder);
    Builder* builderCopy = builder->copy();
    return 0;
}

```