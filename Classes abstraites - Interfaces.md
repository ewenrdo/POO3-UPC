Classes abstraites
==========================

Une classe abstraite est une superclasse qui définit uniquement la forme à partager entre toutes ses sous-classes.

**Nécessaire quand** une superclasse est incapable de créer une implémentation significative pour une méthode.


Elle ne peut pas être directement instanciée, c'est une base de code.
On la définit avec ``abstract class A {}``

Elle ne propose pas forcément d'implémentation, _par exemple_ :
```java
public abstract class Figure {

    private double dimension;
    
    public Figure (double dim) {
        if(dim <= 0) System.out.println("La dimension doit être positive.");
        dimension = dim;
    }
    
    public double getDimension(){
        return dimension;
    }
    
    public abstract double perimetre(); // Méthode abstraite, à implémenter dans les classes dérivées
    public abstract double aire();
}
```

❗ Une classe qui comporte une méthode abstraite est implicitement abstraite. (donc une sous-classe qui ne définit pas toutes les méthodes abstraites est abstraite)
Peut avoir un constructeur, réutilisé par les sous-classes.

**Intérêt :**
- Définir des méthodes communes qui peuvent être partagées par plusieurs sous-classes.
- Imposer l'implémentation de certaines méthodes dans les sous-classes.
- Avoir un mécanisme de réutilisation du code tout en laissant de la flexibilité sur certaines méthodes.


**Une classe ne peut hériter que d'une seule classe abstraite.**


Interfaces
=====

Sert à indiquer ce qu'une classe doit faire, sans indiquer comment.
Permet de séparer l'interface d'une classe de son implémentation.

C'est une **collection de méthodes abstraites** (leur signature)
Elle peut contenir des constantes et classes internes.

Un contrat est une sorte d'étiquette / de contrat, qu'une classe peut implémenter.
**Une classe peut implémenter plusieurs interfaces** avec ``class A implements Interface1, Interface2 {}``

**Interfaces de Java :** ``Comparable<Type>``, ``Cloneable``
Les méthodes statiques doivent toujours avoir une implémentation, même dans les interfaces.

On peut définir des méthodes privées dans l'interface, qui servent de 'helpers' pour ne pas dupliquer du code. Elles ne sont visibles que dans l'interface.

On peut aussi proposer une implémentation par défaut :
```
public interface Collection {
    int taille(); // Méthode abstraite (sans définition)

    default boolean estVide() { // Méthode avec définition par défaut
        return taille() == 0;
    }
```

Les classes qui implémentent l'interface héritent des méthodes par défaut, ce qui signifie qu'elles ne sont pas obligées de les redéfinir.
❗ Les champs d'une interface sont forcément des constantes (``public static final``)
