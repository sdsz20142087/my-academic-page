---
title: Design Pattern --- Structural Pattern
date: 2022-04-27T10:03:37.243Z
summary: "***Facade Pattern, Composite Pattern, Decorator Pattern, Adapter
  Pattern, Flyweight Pattern, Proxy Pattern, Bridge Pattern***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Facade Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that 
//makes the subsystem easier to use. Wrap a complicated subsystem with a simpler interface.

class Subsystem_A{
public:
    void do_operation() {cout << "Subsystem A run" << endl;}
};

class Subsystem_B{
public:
    void do_operation() {cout << "Subsystem B run" << endl;}
};

class Subsystem_C{
public:
    void do_operation() {cout << "Subsystem C run" << endl;}
};

class Facade{
public:
    Facade() {
        A = new Subsystem_A();
        B = new Subsystem_B();
        C = new Subsystem_C();
    }
    virtual~ Facade() {
        delete A;
        delete B;
        delete C;
    }
    void do_operation() {
        B->do_operation();
        A->do_operation();
        C->do_operation();
        return ;
    }

private:
    Subsystem_A* A;
    Subsystem_B* B;
    Subsystem_C* C;
};

int main() {
    Facade facade;
    facade.do_operation();
    return 0;
}

```

## 2. Composite Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Build a complex object out of elemental objects and itself like a tree structure.

class Component{
public:
    Component(string name = "") : _name(name) {}
    virtual Component* AddChild(Component* child) = 0;
    virtual void output(int front) = 0;
protected:
    string _name;   
};

class Composite : public Component{
public:
    Composite(string name = "", int ind = 0) : Component(name), cnt(ind) {}
    virtual~ Composite() {
        for (auto x : childstructure) delete x;
    }
    Component* AddChild(Component* child) override {
        childstructure.push_back(child);
        cnt++;
        return child;
    }
    void output(int front) override {
        for (int i = 0; i < front; i++) cout << "  " << "";
        cout << _name << endl;
        for (auto x : childstructure) {
            x->output(front + 1);
        }
        return ;
    }

private:
    vector<Component*> childstructure;
    int cnt;
};

class Leaf : public Component{
public:
    Leaf(string name = "") : Component(name) {}
    Component* AddChild(Component* child) override {
        cout << "this is Leaf Node, cannot add child Node" << endl;
        return child;
    }
    void output(int front) override {
        for (int i = 0; i < front; i++) cout << "  " << "";
        cout << _name << endl;
        return ;
    }
};

int main() {
    Component* root = new Composite("root");
    Component* midlayer1 = root->AddChild(new Composite("midlayer1"));
    Component* midlayer2 = root->AddChild(new Composite("midlayer2"));
    Component* midlayer3 = root->AddChild(new Leaf("midlayer3_leaf"));
    Component* leaflayer1 = midlayer1->AddChild(new Composite("leaflayer1"));
    leaflayer1->AddChild(new Leaf("leaf1"));
    midlayer2->AddChild(new Leaf("leafl2"));
    midlayer2->AddChild(new Leaf("leafl3"));
    midlayer2->AddChild(new Leaf("leafl4"));
    midlayer3->AddChild(new Leaf("leafl5"));
    root->output(0);
    return 0;
}

```

## 3. Decorator Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//add additional features or behaviors to a particular instance of a class, 
//while not modifying the other instances of same class

class Component{
public:
	Component(int n) : _cost(n) {}
	virtual~ Component() {cout << "Component" << endl;}
	virtual int getcost() = 0;
protected:
	int _cost;
};

class ConcreteComponent : public Component{
public:
	ConcreteComponent(int n = 1) : Component(n) {}
	virtual~ ConcreteComponent() {cout << "ConcreteComponent" << endl;}
	int getcost() override{
		return _cost;
	}
};

class Decorator : public Component{
public:
	Decorator(Component* comp, int n) : Component(n), _comp(comp) {}
	virtual~ Decorator() {cout << "Decorator" << endl;}
	virtual int getcost() = 0;
protected:
	Component* _comp;
};

class ConcreteDecoratorA : public Decorator{
public:
	ConcreteDecoratorA(Component* comp = nullptr, int n = 1) : Decorator(comp, n) {}
	virtual~ ConcreteDecoratorA() {delete _comp; cout << "destructor ConcreteDecorator A" << endl;}
	int getcost() override{
		return _cost + _comp->getcost();
	}
};

class ConcreteDecoratorB : public Decorator{
public:
	ConcreteDecoratorB(Component* comp = nullptr, int n = 2) : Decorator(comp, n) {}
	virtual~ ConcreteDecoratorB() {delete _comp; cout << "destructor ConcreteDecorator B" << endl;}
	int getcost() override{
		return _cost + _comp->getcost();
	}
};

int main() {
	Component* source = new ConcreteComponent(2);
	Component* with_decorator = new ConcreteDecoratorA(new ConcreteDecoratorB(source, 3), 5);
	cout << with_decorator->getcost() << endl;
	delete with_decorator;
	return 0;
}
```

## 4. Adapter Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Convert the existing interfaces to a new interface to achieve compatibility and reusability of the unrelated classes
//in one application. Also known as Wrapper pattern.

class Target{
public:
    Target() {}
    virtual void electricity(int live, int null, int earth) {
        cout << "Three lines electricity!" << endl;
        return ;
    }
};

class Adaptee{
public:
    Adaptee() {}
    void electricity(int live, int null) {
        cout << "Two lines electricity!" << endl;
        return ;
    }
};

class Adapter : public Target{
public:
    Adapter(Adaptee* adaptee = nullptr) : _adaptee(adaptee) {}
    virtual~ Adapter() {delete _adaptee;}
    void electricity(int live, int null, int earth) override{
        _adaptee->electricity(live, null);
        return ;
    }

protected:
    Adaptee* _adaptee;
};

int main() {
    Adaptee* adaptee = new Adaptee();
    Target* target1 = new Target(), *target2 = new Adapter(adaptee);
    target1->electricity(0, 1, 2);
    target2->electricity(0, 1, 2);
    return 0;
}

```

## 5. Flyweight Pattern

```cpp
#include <bits/stdc++.h>
//Make instances of classes on the fly to improve performance efficiently, like individual characters or icons on the screen.

using namespace std;
class Flyweight{
public:
    virtual void doOperation(int extrinsicState) = 0;

};

class ConcreteFlyweight : public Flyweight{
public:
    ConcreteFlyweight(string intrinsicState = "") : _intrinsicState(intrinsicState) {}
    void doOperation(int extrinsicState) override {
        cout << &_intrinsicState << " : do " << extrinsicState << endl;
        return ;
    }
private:
    string _intrinsicState;
};

class FlyweightFactory{
public:
    FlyweightFactory() {}
    virtual~ FlyweightFactory() {
        for (auto x : m) {
            delete x.second;
        }
    }
    Flyweight* getinstance(string name) {
        if (m.find(name) == m.end()) m[name] = new ConcreteFlyweight(name);
        return m[name];
    }

private:
    unordered_map<string, Flyweight*> m;
};

int main() {
    FlyweightFactory* flyweightfactory = new FlyweightFactory();
    flyweightfactory->getinstance("1")->doOperation(1);
    flyweightfactory->getinstance("1")->doOperation(2);
    flyweightfactory->getinstance("2")->doOperation(2);
    delete flyweightfactory;
    return 0;
}

```

## 6. Proxy Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Use a simple object to represent a complex one or provide a placeholder for another object 
//to control access to it.

class Internet{
public:
    virtual void HttpAccess(string url) = 0;

};

class Modem : public Internet{
public:
    Modem(string password) : _password(password) {
        if (password != "123456") cout << "false" << endl;
        else cout << "true" << endl;
    }
    void HttpAccess(string url) override{
        cout << "successful surfing" << endl;
        return ;
    }
private:
    string _password;
};

class Proxy : public Internet{
public:
    Proxy(string password = "123456") {
        _modem = new Modem(password);
        _blackList.push_back("computer");
        _blackList.push_back("game");
        _blackList.push_back("music");
    }
    virtual~ Proxy() {delete _modem;}
    void HttpAccess(string url) override{
        for (auto x : _blackList) {
            if (url == x) {
                cout << "Invalid Url" << endl;
                return ;
            }
        }
        _modem->HttpAccess(url);
        return ;
    }

private:
    Modem* _modem;
    vector<string> _blackList;
};

int main() {
    Proxy proxy;
    proxy.HttpAccess("art work");
    proxy.HttpAccess("game");
    return 0;
}

```

## 7. Bridge Pattern

```cpp
#include <bits/stdc++.h>
using namespace std;
//Decouple an abstraction or interface from its implementation so that the two can vary independently.

class TV{
public:
    virtual void TurnOn() = 0;
    virtual void TurnOff() = 0;
};

class SonyTV : public TV{
public:
    void TurnOn() {
        cout << "SongTV is turning on !" << endl;
    }
    void TurnOff() {
        cout << "SongTV is turning off !" << endl;
    }
};

class LenovoTV : public TV{
public:
    void TurnOn() {
        cout << "LenovoTV is turning on !" << endl;
    }
    void TurnOff() {
        cout << "LenovoTV is turning off !" << endl;
    }
};

class RemoteControl{
public:
    RemoteControl(TV* tv) : _tv(tv) {}
    virtual~ RemoteControl() {delete _tv;}
    virtual void do_operation() = 0;
protected:
    TV* _tv;
};

class Mouse1 : public RemoteControl{
public:
    Mouse1(TV* tv) : RemoteControl(tv) {}
    void do_operation() override{
        cout << "Mouse1 control !" << endl;
        _tv->TurnOn();
        _tv->TurnOff();
    }
};

class Mouse2 : public RemoteControl{
public:
    Mouse2(TV* tv) : RemoteControl(tv) {}
    void do_operation() override{
        cout << "Mouse2 control !" << endl;
        _tv->TurnOn();
        _tv->TurnOff();
    }
};

int main() {
    Mouse1 mouse1(new SonyTV());
    mouse1.do_operation();
    cout << "--------------" << endl;
    Mouse1 mouse11(new LenovoTV());
    mouse11.do_operation();
    cout << "--------------" << endl;
    Mouse2 mouse2(new LenovoTV());
    mouse2.do_operation();
    return 0;
}

```