# Character Arithmetic
## Ou encore : _"Pourquoi devrais-je soustraire des lettres ?"_

Mon petit nom, c'est Patrick. Un jour, j'ai vomi sur un chat par maladresse. Mais ce n'est pas le sujet d'aujourd'hui.

Il fut une époque où j'étais dans une belle entreprise française, ni trop grosse, ni trop petite, avec une moyenne d'âge respectable de 40 ans.  
Ce fut une époque sombre de ma vie, puisque je faisais de la maintenance pour une application qu'on aurait pu croire codé le siècle dernier, muni des meilleures technologies à la pointe de la vétusté.  
On m'informa plus tard que celle-ci avait plus ou moins 3 courtes années.  
_Dommage_.

Je me trouvais donc face à mon écran d'ordinateur, un beau mardi du mois d'août.  
Le code dans lequel je me baladais était exempt de la souffrance habituelle que j'avais pu voir jusqu'alors.  
Il s'agissait d'un petit module en [C] fait avec soin. Plutôt propre. Commenté. Un bel ouvrage, en somme.

Avec tous mes indicateurs au vert, je me relâchais tranquillement, m'abandonnant à la chaleur doucereuse de cette belle journée ensolleillée.

Quand soudain, je _le_ vis.

```c
int nb_charact(char c)
{
    int nb;

    switch (c)
    {
        case 'A': nb = 1; break;
        case 'B': nb = 2; break;
        case 'C': nb = 3; break;
        case 'D': nb = 4; break;
        case 'E': nb = 5; break;
        case 'F': nb = 6; break;
        case 'G': nb = 7; break;
        case 'H': nb = 8; break;
        case 'I': nb = 9; break;
        case 'J': nb = 10; break;
        case 'K': nb = 11; break;
        case 'L': nb = 12; break;
        case 'M': nb = 13; break;
        case 'N': nb = 14; break;
        case 'O': nb = 15; break;
        case 'P': nb = 16; break;
        case 'Q': nb = 17; break;
        case 'R': nb = 18; break;
        case 'S': nb = 19; break;
        case 'T': nb = 20; break;
        case 'U': nb = 21; break;
        case 'V': nb = 22; break;
        case 'W': nb = 23; break;
        case 'X': nb = 24; break;
        case 'Y': nb = 25; break;
        case 'Z': nb = 26; break;
        default: nb = 0; break;
    }
    return nb;
}
```

---

Un éclair de désespoir m'envahit. Quel _gâchis_. Tout avait pourtant si bien commencé. Vraiment, la vie ne valait pas la peine d'être vécue.  
Je suppose que la personne ayant fait cela ne se rendait pas compte de la tristesse qu'elle m'infligeait à ce moment précis.  
Si je ne m'étais pas retenu, j'aurai sans doute vomi sur une portée de chatons. Deux fois.

### J'ai donc pris sur moi afin d'expliquer à la machine à café ce qui n'allait pas.

Il est évident que le but de la fonction est de récupérer, depuis un caractère donné, sa valeur correspondante suivant l'ordre alphabétique.  
Cependant, bien que celle-ci soit techniquement correcte, elle brille par son manque de connaissance d'un concept de l'informatique de base :  
_L'arithmétique de caractère_.

Ce qu'on appelle l'_arithmétique de caractère_ (en français dans le texte) ou encore _character arithmetic_ (en anglais dans le texte) est, tout simplement, l'utilisation d'une table de référence de valeurs de caractère.  
Dans une majorité de cas, la table [ASCII] ou l'[UTF8], en fonction des langages.

Évidemment, mon explication, pour le moment, ne rends pas le [schmilblick] plus clair.  
Rangez vos fourches et autres torches pour combattre la bourgeoisie, je vais être plus clair d'un instant à l'autre.  

Pour faire simple, à chaque caractère est assignée une valeur numérique. Ou plutôt, une valeur est attribué pour chaque caractère.  
C'est ensuite à notre spectaculaire machine à ~~visionner des films pornographiques~~ calculer que reviens la tâche d'afficher, en fonction de la valeur, le caractère correspondant.  

Par exemple, dans la table [ASCII], la valeur `65` équivaut à lettre `'A'`. Pour ceux qui ont un jour fait du [C], on peut utiliser la fonction [printf] pour expliciter mon propos.
```c
// %c affiche un caractère et %d affiche un entier

printf("%c", 'A'); // Affiche le caractère A
// $> A
printf("%d", 'A'); // Affiche l'entier A
// $> 65
printf("%c", 65); // Affiche le caractère de valeur 65
// $> A
```

---

On sait donc dès à présent qu'un caractère n'est autre qu'une simple valeur numérique.  
Et puisque les caractères sont techniquement des entiers, on peut donc tout simplement les additionner, les soustraire ou même comparer.  
_Plait-il ?_

Si on reprend la table [ASCII], on peut voir que le caractère `'B'` est de valeur `66`. `'C'` est de valeur `67`. `'D'` est `68`... Je pense que vous avez compris où je veux en venir.  
La fonction précédente irritant ma pauvre rétine devrait en réalité être aussi simple que ce qui suit :

```c
int nb_charact(char c)
{
    // Alternativement : if (c < 'A' || c > 'Z')
    if (!isupper(c))
    {
        return 0;
    }
    return (c - 'A') + 1;
}
```
[isupper] ?

---

Et voilà comment, armé de connaissances supplémentaires, on arrive à une fonction triviale.  
### Évidemment, il est important de commenter afin que la personne suivante ne connaissant pas non plus l'arithmétique de caractère puisse espérer comprendre ce qu'il se passe ici.
Peut-être que le plétore de lignes de la fonction précédente venait d'une personne voulant expliciter au maximum pour ses braves collègues de bureau ?  
Nous ne le saurons sans doute jamais.


[c]: https://en.wikipedia.org/wiki/C_(programming_language)
[ascii]: https://en.wikipedia.org/wiki/ASCII
[utf8]: https://en.wikipedia.org/wiki/UTF-8
[schmilblick]: https://fr.wikipedia.org/wiki/Schmilblick
[printf]: https://linux.die.net/man/3/printf
[isupper]: https://linux.die.net/man/3/isupper
