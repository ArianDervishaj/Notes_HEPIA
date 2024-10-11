# 1. Typage et Affectations

Indiquez pour chaque affectation si elle est **possible** ou **impossible**. Si l’affectation est impossible, expliquez pourquoi.

1. `int a = 10;`
2. `float b = 3.5;`
3. `short c = 32000;`
4. `int d = 1.9f;`
5. `long e = 1000000000000;`
6. `double f = 1.23456f;`
# 2. Paramètres dans les méthodes

Considérez les méthodes suivantes :
```java
public void m1(int i, float f) {}
public void m2(short b) {}
```

Pour chaque appel de méthode suivant, indiquez s'il est **possible** ou **impossible**. Si l’appel est impossible, expliquez pourquoi.

1. `m1(5, 2.5f);`
2. `m1(5, 3);`
3. `m2(100);`
4. `m2(3.14);`
# 3. Tableaux

Écrivez un programme Java qui :

1. Initialise un tableau `int[][] exemple` avec des tailles variables (par exemple, une première ligne de 3 éléments et une deuxième ligne de 2 éléments).
2. Calcule la somme de chaque ligne de ce tableau et stocke les résultats dans un tableau `int[] result`.
3. Affiche le tableau `result` avec les sommes.

Vous pouvez commencer avec le squelette suivant :
```java
int[][] exemple = {
    {1, 2, 3},
    {4, 5}
};

// Complétez le code pour calculer les sommes
int[] result = new int[exemple.length];

```
# 4. Chaînes de caractères (String)

Écrivez un programme qui :

1. Vérifie si une chaîne de caractères est un palindrome.
2. Convertit chaque mot d'une phrase en majuscules.
3. Affiche uniquement les mots qui sont des palindromes.

Vous pouvez utiliser les méthodes suivantes :

- `String toUpperCase()`
- `String[] split(String regex)`
- `char charAt(int index)`
- `int length()`

```java
String phrase = "Laval Anna est une palindrome";

for (String mot : phrase.split(" ")) {
    // Complétez le code pour vérifier si le mot est un palindrome
    // Si oui, affichez le en majuscule
}
```
