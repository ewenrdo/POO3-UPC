Héritage
==========

**Principe fondamental**

Une classe hérite d'une unique _(pour Java)_ autre (``class A extends B``) alors :
- B est une spécialisation de A
- B a tous les attributs et méthodes de A **mais** :
    - on peut en définir de nouveaux
    - on peut modifier les comportements de méthodes/attributs _(e.g. nouveau toString())_

Avantages : réutiliser le code, maintenabilité
Attention : les nouvelles méthodes ne sont pas accessibles par un objet parent

**Constructeur**

- Le constructeur d'une classe dérivée prend en charge **toute la construction de l'objet**.
- Une sous-classe n'a pas accès aux champs privés de la classe parent.
- On peut appeler le constructeur de la classe parent **(immédiatement supérieure)** avec ``super(args)``.

**Redéfinition d'un champ**
Un champ avec le même nom qu'un champ de la super-classe le cache (qui reste utilisable via ``super.champ``).

**Redéfinition de méthodes**

La rédéfinition permet :
- de personnaliser le comportement de la méthode
- le **polymorphisme** _(i.e. permettre aux objets de sous-classes différentes d'être traités de manière uniforme tout en exécutant des comportements spécifiques à leur type.)

**On peut utiliser @Override pour indiquer clairement qu'on redéfinit une méthode** _(pratique pour les erreurs)_

❗ Pour redéfinir une variable, c'est le nom qui compte. Pour une méthode, c'est sa signature, son nom et le type des paramètres. 
Le type de retour d'une méthode peut être redéfini, mais doit être un sous-type de l'original.

❗ On ne peut pas modifier le niveau d'accès en redéfinissant une méthode/variable (mais on peut l'augmenter). **On peut empêcher la redéfinition en rendant la méthode ``final``**.

**Surcharge de méthodes**

Si le nom d’une méthode est le même, mais pas la signature, on ne parle pas de redéfinition (overriding), mais de surcharge (overloading).
--> Une classe dérivée peut surcharger une méthode d'une classe de base
--> On peut totalement changer le type de retour

**Polymorphisme**
Un objet d'une sous-classe est également un objet de sa super-classe.
Il possède donc deux types :
- **Type déclaré** : La superclasse (ne change pas, déterminé à la compilation)
- **Type effectif** : La classe avec laquelle l'objet est instancié (change dynamiquement)
--> La méthode ``.getClass()`` renvoie le type effectif de l'objet

Donc un objet peut être référencé par des variables de tout type supérieur ou égal à son type effectif.
On peut utiliser ``instanceof ...`` pour tester si un objet descend d'une classe

_exemple :_
```
class A {}
class B extends A {}
class C extends B {}

B c = new C();  // c a un type déclaré B mais un type effectif C
```

**Compilateur**
Le compilateur ne reconnaît que le type déclaré.

**Dynamic binding** : processus par lequel la JVM détermine (quand on exécute) la méthode à invoquer sur un objet en fonction de son type. C'est le type effectif qui détermine la définition à utiliser.
❗ **Pas vrai pour les variables ou les méthodes ``static`` !** Pour elles, on a le static binding. C'est la variable correspondant au type déclaré qui est utilisée. _(donc redéfinir un champ != redéfinir une méthode)_

```
class A {
    int c = 1;
    public int getC () {
    return c; // c de A: determiné à la compilation
  }
}

class B extends A {
    int c = 2; 
}

B b = new B();
System.out.println(b.c); // affiche 2
System.out.println(b.getC()); // affiche 1
```

Et comme c'est déterminé à la compilation, on a ce genre de choses :
```
class A {
    int c = 1;
    public int getC () {
        return c; // c de A: determiné à la compilation
    }
}
    
class B extends A {
    int c = 2;
    
    @Override
    public int getC () {
        return c; // c de B: determiné à la compilation
    }
}

B b = new B(); 
A ab = b;
System.out.println(b.getC()); // affiche 2
System.out.println(ab.getC()); // affiche 2
```

**Transtypage (casting)**

Pour utiliser un objet entièrement il est possible de forcer le changement de son type déclaré.

```
Personne p = new Employe("Mickey Mouse", 95, "Disney");
Employe e = (Employe) p;
```
Alors ``e`` est de type déclaré ``Employe``.

**Casting autorisé ssi les types déclarés sont dans une même hiérarchie d'héritage.**
- L'upcasting est toujours sûr (et même implicite quand on fait ``Personne p = e`` ~ ``Personne p = (Employe) e``
- Le downcasting est sûr que s'il ne dépasse pas le type effectif. _(toujours tester avec instanceof)_

**Ordre d'initialisation**
- Un objet de C (avec C extends B extends A) contient une partie A, une partie B, puis C.
- À la construction, on initialise du parent vers l’enfant :
    1. Allocation de l’objet → champs à leurs valeurs par défaut (0, false, null).
    2. On démarre par le constructeur le plus dérivé (C(...)).
       - S’il commence par this(...), on suit la chaîne interne de C jusqu’au constructeur de C qui n’appelle pas this(...).
       - Ce “dernier” constructeur de C appelle alors super(...) (implicite ou explicite).
     3. On monte niveau par niveau : appel d’un constructeur de B, puis de A, etc.

**Classes scellées** 
``public sealed class Shape permits Circle, Square, Rectangle {}`` : Seules Circle, Square et Rectangle peuvent hériter de Shape.

❗❗ **Toute classe hérite (implicitement) de ``Object``.
