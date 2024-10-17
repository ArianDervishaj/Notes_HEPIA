# 1. Typage (5 pts)

Indiquez, pour chaque affectation dans les lignes ci-dessous si l’affectation est possible, et le cas échéant, la valeur exacte qui sera affectée à la variable.

| Ligne |           Code           | Possible : oui – non | Valeur |
| :---: | :----------------------: | :------------------: | :----: |
|   1   |      `double a1 = 3;`      |                      |        |
|   2   |      `int a2 = 2.4;`       |                      |        |
|   3   |   `int a3 = (int) 4.3;`    |                      |        |
|   4   |      `short a4 = 12;`      |                      |        |
|   5   |     `float a5 = 4.44;`     |                      |        |
|   6   | `double a6 = (double) a4;` |                      |        |
|   7   |  `double a7 = (int) a6;`   |                      |        |
|   8   |     `float a8 = 5.5F;`     |                      |        |
|   9   |    `int a9 = a8 / a7;`     |                      |        |
|  10   |    `int a10 = 45 / 4;`     |                      |        |
# 2. Paramètres de Méthodes (5 pts)

On dispose des méthodes et des déclarations suivantes :

```java
1. public void m1(int i, float f) {}
2. public void m2(short b) {}
3. int i;
4. short s;
5. float f;
6. double d;
```

Spécifiez si les appels suivants sont corrects ou non tout en justifiant vos réponses.

| Ligne |       Appel       | Correct ? | .                          Pourquoi ?                          . |
| :---: | :---------------: | :-------: | :--------------------------------------------------------------: |
|  11   |    `m1(4, d);`    |           |                                                                  |
|  12   |    `m1(i, f);`    |           |                                                                  |
|  13   |   `m1(s+4, f);`   |           |                                                                  |
|  14   | `m1(i, 3.2 * f);` |           |                                                                  |
|  15   |   `m1(7/6, d);`   |           |                                                                  |
|  16   |     `m2(10);`     |           |                                                                  |
|  17   |     `m2(128)`     |           |                                                                  |
|  18   |     `m2(s);`      |           |                                                                  |
|  19   |    `m2(6.0);`     |           |                                                                  |
|  20   |    `m2(s+s);`     |           |                                                                  |
# 3. Tableaux (5 pts)

Voici un programme qui déclare, initialise et manipule un tableau de tableaux de tailles variables (`int[][] exemple`) et qui affiche un tableau résultat (`int[] result`) contenant la somme de chaque ligne de `exemple`.

```java
public class CalculeSomme {

    public static void main(String[] args) {
        int[][] exemple = {
            { 1, 5, -3, 10 },
            { 2, -2, 5, -6},
            { 1, 3, 3 }
        };

        int[] result = somme(exemple);
        System.out.println();
        System.out.print("La somme des éléments de chaque lignes du tableau est : ");
        for (int elem : result) {
            System.out.print(elem + " ");
        }
        System.out.println();
    }

    public static int[] somme(int[][] tab) {
        // A compléter
    }
}
```

L’exécution de ce code doit donner comme résultat :

**La somme des éléments de chaque lignes du tableau est : 13 -1 -7**