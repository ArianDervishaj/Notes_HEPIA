---
title: Héritage
draft: false
tags:
  - POO
---
---
# Définition

- L'héritage est **un mécanisme qui permet de créer une nouvelle classe à partir d'une classe existante.** Cette nouvelle classe **hérite des propriétés et des méthodes de la classe parente** (classe de base).
- **Spécialisation-Généralisation** :
    - Dans la POO, l'héritage est souvent décrit par **les termes de spécialisation et généralisation.**
    - **Généralisation** : La classe de parent représente un concept plus général.
    - **Spécialisation** : La classe dérivée (ou enfant) représente un concept plus spécifique, en ajoutant ou modifiant les comportements.
- **Réutilisation du code** :
	- L'héritage permet de maximiser la réutilisation du code.

---
# Syntaxe en java

```java
public class BankAccount extends Account {
	private Bank bank;

public BankAccount(String owner, Bank bank){
	super(owner, 0.0); // Appelle le constructeur de la classe parent 
	this.bank = bank;
	}

	public Bank getBank() {return this.bank;}
}
```

> [!Info] super()
> Si on appelle pas super() dans le constructeur, java le fera automatiquement avec aucun argument.

---
# Ajouts de méthodes

Une classe **dérivée** (classe enfant) peut :
- Ajouter des nouvelles méthodes
- Surcharger des méthodes héritées
- Redéfinir des méthodes héritées
- Peuvent modifier la visibilité des méthodes hérités

```java
public class CryptoAccount extends Account {
    private List<String> providers = new ArrayList<>();

    public CryptoAccount(String owner, double balance) {
        super(owner, balance);
    }

    public CryptoAccount(String owner) { 
        this(owner, 0.0); 
    }

// Inexistantes dans la super classe Account
    public void addProvider(String provider) { 
        this.providers.add(provider);
    }

    public void showProviders() {
        for(String provider: providers) {
            System.out.println(provider);
        }
    }
}
```
## Surcharger des méthodes

Utiliser un nom de méthode héritée mais **avec des paramètres différents**.
Permet d'**ajouter une nouvelle méthode même si le nom existe**.

```java
// Account.java
public class Account {
    // ...
    public void withdraw(double amount) {
        // Code 1
    }
}
--------------------------------------------------------------------------------------------------------------
// CryptoAccount.java
public class CryptoAccount extends Account {
    // ...

    public void withdraw(double amount, int Commission) { //Surchage la fonction de Account
        // Code 2
    }
}

```
## Redéfinition

**La redéfinition remplace l'implémentation interne** d'une méthode existante héritée

```java
// Account.java
public class Account {
    // ...
    public void withdraw(double amount) {
        // Code 1
    }
}
---------------------------------------------------------------------------------------------------------------
// CryptoAccount.java
public class CryptoAccount extends Account {
    // ...
    public void withdraw(double amount) {
        // Code 2 != Code 1
    }
}
```

Lors de l’exécution, c’est toujours **le [[Héritage#Types réels vs Types déclarées|type réel]] et non pas déclaré qui est considéré.**
Donc la méthode utilisé, sera celle du **type réel**.

---
# Utiliser les méthodes de la super classe

Utiliser **le mot clé super**.

```java
class A {
    public String foo() { return "I'm A";}
}

class B extends A {
    @Override
    public String foo() { return super.foo() + " and I'm B"; }
}

B b = new B();
b.foo(); // return "I'm A and I'm B"

```

---
# Lien entre constructeurs

Lors d’un héritage, il faut faire **attention à l’initialisation des attributs hérités**, surtout **ceux qui sont privés**, car **ils ne sont pas accessibles dans la sous classe.**

```java
-----------------------------------------------------------------------------------------------------------------
// Point.java
class Point {
    private double x, y;
    public Point() {
        x = Math.random() * 100;
        y = Math.random() * 100;
    }
    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }
    public void add(int dx, double dy) {
        this.x += dx;
        this.y += dy;
    }
    public double getX() { return x; }
    public double getY() { return y; }
}
-------------------------------------------------------------------------------------------------------------
//ColoredPoint.java
class ColoredPoint extends Point {
    private byte color;

    public void setColor(byte c) { color = c; }
    public byte getColor() { return color; }
}
```

**Un constructeur de la classe point est appelé dans lorsqu'un objet `ColoredPoint` est créé.**

```java
class ColoredPoint extends Point {

    // Choix 1
    public ColoredPoint() {
        color = (byte)(Math.random() * 256);
    }

    // Choix 2
    public ColoredPoint() {
        super();
        color = (byte)(Math.random() * 256);
    }

    // Choix 3
    public ColoredPoint() {
        super(0, 0); // Choisit le constructeur Point(double x, double y)
        color = (byte)(Math.random() * 256);
    }
}
```

Lorsqu'on créé un objet d'une classe, sans spécifier explicitement quel constructeur de la classe parente utiliser, Java va automatiquement appeler le **constructeur par défaut** de la classe parente. (Choix 1 et 2)
Le super() dans le choix 2 ne sert à rien.

> [!Warning] Constructeur avec super()
> Si on utilise le mot clé `super()` pour appeler le constructeur du parent. Il faut que ce soit la première instruction du construteur.

> [!Warning] Héritage des constructeurs
> Les constructeurs ne sont pas héritées. Il faut avoir un constructeur pour chaque classe. 
> Si aucun constructeur n'est définit, Java en définit un par défaut qui ne fait rien.

---
# Typage entre classes

```java
// déclarée dans le fichier A.java
public class A {  
    protected String nom = "Je suis dans A";

    public void uneMethode() {
        System.out.println(nom);  // imprime "Je suis dans A"
    }
}
----------------------------------------------------------------------------------------------------------
// déclarée dans le fichier B.java

public class B extends A {  
    protected String nom = "Je suis dans B";

    public void uneAutreMethode() {
        System.out.println(nom);       // imprime "Je suis dans B"
        System.out.println(super.nom); // imprime "Je suis dans A"
    }
}
```

> A super classe - Type A.
> B sous classe   - Type B est un Sous type de A.

**On peut affecter une variable d'un sous type dans une variable d'un super type.**
```java
public class Main {  // déclarée dans le fichier Main.java

    public static void main(String... args) {
        A a = new A();  // la classe A est celle que nous venons de définir
        B b = new B();  // idem
        A ba = b;       // cette déclaration est légale, car b est un élément de A

        System.out.println("a.nom = " + a.nom);
        System.out.println("b.nom = " + b.nom);
        System.out.println("ba.nom = " + ba.nom);
    }
}
```
## instanceOf

Permet de tester le type d'une variable complexe.

```java
public class TableModel2 extends TableModel1 {
    // code
}

TableModel2 tableModel = new TableModel2();

boolean t2 = tableModel instanceof TableModel2; // True
boolean t1 = tableModel instanceof TableModel1; // True
```

> [!Warning] Attention
> une variable de type B, est aussi de type A si B hérite de A.
## Comment différencier le type ?

```java
boolean t2 = tableModel.getClass().equals(TableModel1.class); // False
boolean t2 = tableModel.getClass().equals(TableModel2.class); // True
```
---
# Interdire l'héritage

Utilise le mot clé `final`.
```java
public final class A {  // déclarée dans le fichier A.java, ne peut être étendue
    public void ouSuisJe() {
        System.out.println("Je suis dans A !");
    }
}
------------------------------------------------------------------------------------------------------------
public class B {  // déclarée dans B.java
    public final void ouSuisJe() {  // ne peut être surchargée, bien que B puisse être étendue
        System.out.println("J'y suis j'y reste !");
    }
}
```

--- 
# Types réels vs Types déclarées

```java
Account A; // A est de type déclaré Account
A = new BankAccount(); // A est de type réel BankAccount
A = new CryptoAccount(); // A est de type réel CryptoAccount

```