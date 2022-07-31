# Operators Overloading. Operator types.

### *Informații generale*

  * Supraîncărcarea operatorilor funcționează similar cu implementarea unei funcții sau a unei metode. 

### *Sintaxă*

```c++
return_type operator operator_symbol(parameters);
```

Exemple:

```c++ 
Date operator+(const Date&); 
friend ostream& operator <<(ostream&, const Date&);
```
-----
### ***Unary operators***
<br>

#### **1. Diferența dintre postfix și prefix increment/decrement.**

prefix increment | postfix increment
-----------------|------------------
++var            | var++ 

<br>

În practică, ambele aduc la același rezultat, însă trebuie ținut cont de faptul că trebuie aleasă varianta care nu presupune crearea unei copii pentru obiectul de referință. Funcționează la fel atât pentru increment, cât și pentru *decrement*.

```c++
    Date operator++() { /// prefix increment
        this->day++;
        return *this;
    }


    Date operator++(int value) { ///postfix increment
        Date copie(*this); /// Date copie(this->day, this->month, this->year);
        copie.day++;

        return copie;
    }
```

#### **2. Conversion operators**

Keyword-ul `explicit` adăugat la declararea unui operator ne ajută în a scăpa de eventuale erori ale compilatorului. El poate să fie folosit de către:

* constructori; sau
* operatori de conversie. 

Folosit la introducerea unui constructor, keyword-ul `explicit` stabilește că obiectul construit cu ajutorul acelui constructor nu poate să fie folosit ca parametru într-un constructor de copiere. 

```c++
    explicit operator int() {
        return (this -> year * 100) + (this -> month *100) + (this -> day);
    }

    explicit Date(int dayParam)  day(dayParam) { }
```

#### **3. Deference Operator (*) and member Selection Operator (->)**

Cei doi operatori sunt folosiți în cele mai multe cazuri împreună cu smart pointer classes.

<span style="color: #ff4d4d">*Definiție*:</span> Un smart pointer este o clasă cu opeartori supraîncarcați, care se comportă ca un pointer. Acest pointer are abilitate de a se asigura de buna funcționare a aplicației, luând în calcul alocarea dinamică a memoriei.


Cu alte cuvinte, un smart pointer descrie o clasă care simplicifă folosirea memoriei cu ajutorul pointerilor normali. În unele cazuri, **pot să ajute la imbubătățirea performanței aplicației**.


```c++
/// pentru folosirea acestori smart pointeri folosim:
#include <memory> /// din care folosim unique_ptr.

int main() {
    unique_ptr<Date> myDate = Date(15,05,2021);
    myDate->displayDate();

    return 0;
}
```

Q: **De ce am opta pentru folosirea unui smart pointer in comparație cu un obiect?**

A: **Un smart pointer poate să facă mai multe lucruri decât un pointer normal, fiind o alegere mai bună când vine vorba de alocarea memoriei.**

---

### ***Binary operators***

<br>

#### **1. Definiție**
<span style="color: #ff4d4d">*Definiție*:</span> Operatorii binari presupun implicarea a două elemente în operație.

<br>

#### **2. Exemple de operatori binari:**
Tabelul de mai jos descrie câteva exemple de operatori binari:

Operator | Usage
---------|--------
*, +, -, /, % | Operații matematice
+=, *=, -=, /=, %= | Operații matematice + atribuire
= | Operatorul de atribuire 
==, != | Operatorul de egalitate și cel de inegalitate
[] | Subscript operator

<br>

#### **3. Operatii matematice cu sau fără atribuire**

Principiul este asemănător cu cel folosit în *postfix operations*, astfel folosindu-se un obiect auxiliar. Funcționează **similar** pentru **adunare și scădere**.

Asemănător cu *prefix operations* se implementează **operațiile matematice cu atribuire**, în ideea că **NU** necesită un obiect auxiliar.

```c++
    Date operator+ (int tobeAdded) {
        Date copie(this->day+tobeAdded, this->month, this->year); 
        return copie;
    }

    Date operator+ (const Date& addedDate) {
        return Date(this->day+addedDate.day, this->month+addedDate.month, this->year+addedDate.year);
    }

    void operator+= (int addedDay) {
        this->day += addedDay;
    }
```

#### **4. Operatorul de egalitate si cel de inegalitate**

Sunt folositi pentru a verifica dacă două obiecte care aparțin aceleiași clase au aceleași caracteristici.

Implementarea acestora este destul de simplă, verificându-se dacă variabilele implicate în comparare sunt egale. **Inegalitatea** se poate face prin **negarea egalității**.

```c++
    bool operator== (const Date& tobeCompared) {
        return (this->day == tobeCompared.day && this->month == tobeCompared.month && this->year == tobeCompared.year) ? true : false;
    }

    bool operator!= (const Date& tobeCompared) {
        return !(this->operator==(tobeCompared)) ? true : false;
    }

IMPORTANT: Este recomandată folosirea !(this->operator== (tobeCompared)) in detrimentul variantei !(this == tobeCompared) pentru a evita erori cu privire la lucrul cu pointeri.
```

Asemănător se lucrează cu operatorii: **<, >, >=** respectiv **<=**.


#### **Operatorul []**

Este folosit pentru implementarea array-urilor respectiv a matricelor. Acesta poate să aibă o definiție asemănătoare cu:

```c++
operator[] (int index) {
        Node *p = l;
        for(int i = 0; i<index; i++) {
            p = p->address;
        }
        return p->value;
    }
```

Este nevoie de existența unei liste de noduri care să conțină datele stochate de către tablou.


<details>
<summary>Codul scris de mine pentru clasa Date.</summary>

```c++
#include <fstream>
#include <string>
#include <iostream>
using namespace std;

class Date {
    
    protected:
        int day = 0, month = 0, year = 0;
        static string monthArray[12];

    public:
    
        Date() { }
        Date(int day, int month, int year) : day(day) , month(month), year(year) { }
        Date(int day, int month) : day(day), month(month) { }
        explicit Date(int day) : day(day) { }
        Date(const Date& data) : day(data.day), month(data.month), year(data.year) { }

        void displayDate() {
            cout<<Date::monthArray[this->month-1]<<' '<<this->day<<", "<<this->year<<endl;
        }

        int getDay() { return this->day; }
        string getMonth() { return monthArray[this->month]; }
        int getYear() { return this->year; }

        friend ostream& operator<<(ostream&, const Date&);
        explicit operator int(); /// conversie a obiectului Date la int
        Date operator+(const Date& dateAdd); 
        Date operator++(); /// prefix increment
        /// Date operator++(const Date& toBeAddedTo); postfix increment -> presupune crearea unei copii.
        bool operator== (const Date& tobeCompared);
        bool operator!= (const Date& tobeCompared);

        ~Date() { }
};


string Date::monthArray[12] =  {"Ian", "Feb", "Mar", "Apr", "May", "Jun", 
                                "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"};

Date Date::operator+(const Date& addedDate) {
    int objDay = this->getDay() + addedDate.day;
    int objMonth = this->month + addedDate.month;
    int objYear = this->year + addedDate.year;

    if(monthArray[objMonth-1] == monthArray[1]) {
        if(objYear % 4 == 0) {
            if(objDay > 29) { 
                objDay -= 29;
                objMonth++;
            }
        } else {
            if(objDay > 28) { 
                objDay -= 28;
                objMonth++;
            }
        }
    } else { 
        if((objMonth-1)%2 == 0){ 
            if(objDay > 31) {
                objDay -= 31;
                objMonth++;
            }
        } else {
            if(objDay > 30 ) { 
                objDay -= 30;
                objMonth++;
            }
        }
    }

    if(objMonth > 12 ) {
        objMonth -= 12;
        objYear ++;
    }

    return Date(objDay, objMonth, objYear);
}

Date Date::operator++() {
    this->day++;
    int objDay = this->getDay();
    int objMonth = this->month;
    int objYear = this->year;

    if(monthArray[objMonth-1] == monthArray[1]) {
        if(objYear % 4 == 0) {
            if(objDay > 29) { 
                objDay -= 29;
                objMonth++;
            }
        } else {
            if(objDay > 28) { 
                objDay -= 28;
                objMonth++;
            }
        }
    } else { 
        if((objMonth-1)%2 == 0){ 
            if(objDay > 31) {
                objDay -= 31;
                objMonth++;
            }
        } else {
            if(objDay > 30 ) { 
                objDay -= 30;
                objMonth++;
            }
        }
    }

    if(objMonth > 12 ) {
        objMonth -= 12;
        objYear ++;
    }

    return *this;
}

ostream& operator<<(ostream& out, const Date& outDate) {
    return (out<<Date::monthArray[outDate.month-1]<<' '<<outDate.day<<", "<<outDate.year);
}

Date:: operator int()  {
    return (this -> year * 100) + (this -> month *100) + (this -> day);
}

bool Date::operator== (const Date& tobeCompared) {
    return (this->day == tobeCompared.day && 
            this-> month == tobeCompared.month && 
            this->year == tobeCompared.year) ? true : false;
}

bool Date::operator!= (const Date& tobeCompared) {
    return !(this->operator==(tobeCompared)) ? true : false;
}

void Date:: operator+= (int value) {
    this->day += value;
}

int main() {

    if(Date(15,05,2021) == Date(16,05,2021)) cout<<1;
    else cout<<0;
    return 0;
}
```
</details>