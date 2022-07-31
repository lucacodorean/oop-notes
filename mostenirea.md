# Object Oriented Programming


## Moștenirea (Inheritance)

### _Definiție_

* Moștenirea este o relație care se stabilește între **cel puțin două** clase (clasa de bază respectiv clase derivate). Prin intermediul acestei relații, clasa derivată devine **o particularitate** a clasei de bază.

* Clasele derivate pot să aibă la rândul lor clase derivate din acestea.
* Clasele de bază își generează obiectele înaintea claselor derivate. Procesul funcționează exact *invers* în cazul destructorilor.
--- 

### _Sintaxă_

Sintaxa pe care o clasă derivată o respectă este asemănătoare cu:

```c++
class Base {
    @base class definition
};

class Derived : access-specifier Base {
    @derived class definition
};
```
<span style="color:#ff4d4d;">__access-specifier__</span> poate să aibă următoarele valori:
* _public_ = clasa derivată poate să fie o clasă de bază.
* *private* sau _potected_ = clasa derivată **ARE** o clasă de bază. 
  
<br>

Modificator de acces la membrii din base class | Accesul moștenit **public** | Accesul moștenit **private** | Accesul moștenit **protected**
--------|----|-----------|---------------------------
**public** |  <span style="color:#ff4d4d;">*public*</span>  | <span style="color:#ff4da6;">*private*</span>  | <span style="color:#ff4da6;">*private*</span>                    |    
**private** | <span style="color:#ff4da6;">*private*</span>  | <span style="color:#ff4da6;">*private*</span>  | <span style="color:#ff4da6;">*private*</span>  |
**protected** | <span style="color:#00b8e6;">*protected*</span>  | <span style="color:#ff4da6;">*private*</span>  | <span style="color:#00b8e6;">*protected*</span>  |

<br>

*<span style="color:#ff4d4d;"><u>Important:</u></span>* Moștemirea <span style="color:#00b8e6;">***protected***</span> permite clasei derivate dintr-o altă clasă derivată să acceseze membrii publici sau protected ai clasei de bază inițiale.

Prin moștenirea <span style="color:#ff4da6;">***privată***</span> toate metodele publice ale clasei de bază devin private. Totuși, acestea pot sa fie folosite în interiorul metodelor din clasa derivată.   

-----

### _Base class intialization. Passing parameters to the base class._
<br>

#### Q: Când folosim base class initalization? 

A: Un astfel de mecanism este extrem de folositor în situația în care clase derivată depinde de anumite câmpuri din clasa de bază, folosind inițializarea pentru a completa anumite câmpuri din clasa be bază.

```c++
class Bird {
    protected:
        string name;
        bool canFly = false;
    
    public:
        Bird() { }
        Bird(string nameParam, bool abletoFly) : name(nameParam), canFly(abletoFly) { }
};

class Parrot final: public Bird {

    private:
        string color;

    public: 
        Parrot() { }
        Parrot(string paramName, string color) : Bird(paramName, true) {
            this->color = color;
            type = 2;
        }
};
```

----

### *Derived class overriding base class's methods*
<br>

*<span style="color:#ff4d4d;"><u>Definiție:</u></span>* 
Dacă în clasa derivată apare o funcție declarată în același mod ca și în clasa de bază, apare fenomenul de **supra-scriere** a metodei din clasa de bază.

Astfel, la apelul metodei de către clasa derivată, se va executa setul de instrucțiuni scris în clasa derivată și nu în cea de bază. Totuși, în cazul în care dorim să folosim metoda scrisă în clasa de bază, putem să apelăm la **SCOPE RESOLUTION OPERATOR – ::**.

```c++
class Bird {
    public:
        @constructori 
        void displayText() { cout<<"This is a text."; }
};

class Parrot final: public Bird {

    private:
        string color;

    public: 
        @constructori
        void displayText() { cout<<color; }
};

int main() {
    Parrot papagal = Parrot("Sunny", "red");

    papagal.displayText(); /// afisează: red;
    papagal.Bird::displayText(); /// afișează: This is a text. 
    return 0;
}
```

De asemenea, cu ajutorul aceluiași operator se pot apela metode suprascrise din clasa de bază.

```c++
class Parrot final: public Bird {

    private:
        string color;

    public: 
        @constructori
        void displayText() { Bird::displayText(); }
};
```

----

### *Multiple Inheritance*

<br>
C++ permite unei clase derivate să moștenească una sau mai multe clase, ele fiind menționate în sintaxă astfel:

```c++
class Derived /*final*/ : access-specifier Base1, access-specifier Base2, ... {
    @derived class definition
};
```

*<span style="color:#ff4d4d;"><u>Important:</u></span>* Începând cu C++11, s-a implementat specifier-ul `final` care este folosit cu scopul de a declara o clasă care nu poate să mai fie folosită drep clasă de bază. 

--- 

Mai jos am atașat și codul scris de mine pentru a înțelege mai bine conceptul de moștenire a claselor.

<details>
<summary>Codul scris de mine pentru moștenirea claselor.</summary>

```c++
#include <fstream>
#include <string>
using namespace std;

ofstream cout("inheritance.out");

class Animal {

    protected: 

        bool drinksWater;

    public:
        Animal() { }
        Animal(bool drinksWaterParam) : drinksWater(drinksWaterParam) { }

        void isDrinking() {
            drinksWater == true ? cout<<"(animal class method) Bea apa.\n" : cout<<"(animal class method) Nu bea apa.\n";
        }
};

class Bird {

    protected:

        string name;
        bool isFeathered = false;
        bool hasWings = false;
        bool laysEggs = false;
        bool canFly = false;

    public:
        
        int type = 0;

        Bird() { } 
        Bird(string name) { 
            this->name = name;
            type = 1;
        }

        Bird(string name, bool canFly) : name(name), canFly(canFly) { }

        void setWings() {
            this->hasWings = true;
        }

        string getName() {
            return this->name;
        }

        void displayType() {
            /*switch (type)
            {
            case 1: 
                cout<<getName()<< " este o pasare oarecare.\n";
            case 2:
                cout<<"\n"<<getName()<<" este un papagal.";
            default:
                break;
            }*/

            if(type == 1) cout<<"\n"<<getName()<< " este o pasare oarecare.";
            else cout<<"\n"<<getName()<<" este un papagal.";
        }

        void abletoFly() {
            canFly == true ? cout<<this->getName()<<" poate sa zboare.\n" : cout<<this->getName()<<" NU poate sa zboare.\n";
        }

        void displayColor() {
            cout<<"(base class method) Nu se cunoaste culoarea lui "<<getName()<<'.';
        }

        ~Bird() { }
};

class Parrot final: public Bird, public Animal {
    
    private: 
        string color;

        void isParrot() {
            bool isFeathered = true;
            bool hasWings = true;
            bool laysEggs = true;
            bool canFly = true; 
            type = 2;
        }

    public:
        Parrot() {
            isParrot();
        }

        Parrot(string paramName) {
            name = paramName;
            isParrot();
        }

        Parrot(string paramName, string color) : Bird(paramName, true), Animal(true) {
            this->color = color;
            type = 2;
        }

        void setColor(string color) {
            this->color = color;
        }

        string getColor() {
            return this->color;
        }

        void displayColor() {
            cout<<"(parrot class method) "<<getName()<<" are culoarea "<<getColor()<<".\n";
        }

        ~Parrot() { }
};

void display(Bird param1, Parrot param2);

int main() {
    display(Bird("Sunny"), Parrot("Levy", "rosu"));
    return 0;
}

void display(Bird param1, Parrot param2) {
    ///Bird pasare("Sunny");
    ///Parrot papagal("Buddy", "rosu");
    cout<<"\nDETALII DESPRE PASARI\n\n";
    param1.displayType();
    param2.displayType(); 
    cout<<"\n_______________\n\n";
    param1.abletoFly();
    param2.abletoFly();
    cout<<"\n_______________SUPRASCRIEREA METODELOR\n\n";
    param2.Parrot::displayColor();
    param2.Bird::displayColor();
    cout<<"\n_______________PROTECTED\n\n";
    param2.isDrinking();

    cout.close();
}
```
</details>
