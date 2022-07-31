# Polimorfism

### _Definiție_

* Oferă posiblitatea de a lucra mai flexibil cu metodele claselor moștenite, astfel ajungându-se la rezultate diferite. Lucrează "mână în mână" cu moștenirea și procesele de supraîncărcare a operatorilor, respectiv suprascriere.
----
### **Keyword-ul virtual**

- Keyword-ul `virtual` oferă prioritate funcției din clasa de bază, astfel la apelul acesteia, suprascrierea metodei fiind ingorată. 

- Important: Este indicat să construim constructorii clasei de bază declarându-i `virtual`. Prin acest mod, ne asigurăm că pointerii la clasa de bază nu sunt distruși prin apeluri (greșite) ale destructorilor din clasele derivate.

- Pentru o a treia clasă derivată, ce moștenește clasele Derived1 și Derived2, care moștenesc la rândul lor aceeași clasă de bază, rezultă o singură instanță a clasei de bază în crearea clasei Derived3.

De ce să folosim keyword-ul?

Folosirea keyword-ului `virtual` te asigură că apelul funcției din clasa de bază (objBse.Function) este redirecționat la funcția din clasa derivată.

### **Keyword-ul ovverride**

- Folosirea keyword-ului `override` este o metodă extrem de folositoare prin care putem să ne explicităm intenția de a suprascrie o funcție din clasa de bază. Compilatorul va verifica dacă:
    * funcția din clasa de bază este declarată `virtual`; respectiv
    * cele două funcții au aceeași semnătură.

### **Concluzii** 

![](https://imgur.com/LXMbBGj.png)