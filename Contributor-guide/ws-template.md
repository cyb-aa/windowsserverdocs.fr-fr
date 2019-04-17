---
title:
- TITRE DE L’ARTICLE
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082061"
---
# <a name="metadata-and-markdown-template"></a>Métadonnées et modèle de promotions

Ce modèle d’opérations contient des exemples de syntaxe de promotions, ainsi que des instructions sur la définition de métadonnées. Pour obtenir le meilleur parti, vous devez consulter les [promotions brutes](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) et que le [mode de rendu](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (Les promotions brutes indique le bloc de métadonnées, mais pas l’affichage).

Lorsque vous créez un fichier de promotions, vous devez copier ce modèle dans un nouveau fichier, remplissez les métadonnées comme spécifié ci-dessous, les en-têtes H1 ci-dessus au titre de l’article et supprimer le contenu. Tout en majuscules entre crochets requiert votre attention.


## <a name="metadata"></a>Métadonnées 

Le bloc de métadonnées complètes se trouve en haut. Certaines notes clés:

- Vous **devez** avoir un espace entre le signe deux-points (:)) et la valeur d’un élément de métadonnées.
- Deux-points (par exemple, un titre) une valeur de saut de l’Analyseur de métadonnées. À leur place, utilisez le codage HTML pour un signe deux-points de `&#58;` (par exemple, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **titre**: ce titre apparaît dans les résultats des moteurs de recherche. 
- **auteur**: le champ auteur doit contenir **nom d’utilisateur de référentiels** de l’auteur, pas leurs alias.
- **MS.prod**, **ms.technology**: utiliser «seuil de serveur windows» pour ms.prod (ou w10 si vous utilisez ce modèle pour créer du contenu pour Windows 10). Demandez à votre contact CX pour obtenir la valeur ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Promotions de base, GFM et caractères spéciaux

Toutes les promotions de base et aromatisée référentiels sont pris en charge. Pour plus d’informations, voir:

- [Syntaxe de promotions planifié](https://daringfireball.net/projects/markdown/syntax)
- [Documentation de promotions (GFM) AROMATISEE de référentiels](https://guides.github.com/features/mastering-markdown)

Promotions utilise des caractères spéciaux tels que \ *, \», et \ # pour la mise en forme. Si vous souhaitez inclure un de ces caractères dans votre contenu, vous devez effectuer une des deux choses:

- Placer une barre oblique inverse avant le caractère spécial pour «ignorer» (par exemple, \\\ * pour un \ *)
- Utilisez le [code HTML de l’entité](http://www.ascii.cl/htmlcodes.htm) du caractère (par exemple, \ & \#42\; pour un & #42;).

## <a name="headings"></a>Titres

Titres doivent être effectuées à l’aide du style atx, autrement dit, utiliser des caractères de hachage de 1 à 6 (#) au début de la ligne pour indiquer un titre, correspondant aux niveaux d’en-têtes HTML H1 à H6. Exemples d’en-têtes des premier et deuxième niveau sont utilisés ci-dessus. 

Il **doit** n'être qu’un seul en-tête de premier niveau (H1) dans la rubrique qui sera affichée comme titre sur la page.  

En-têtes de second niveau génère la table des matières sur la page qui s’affiche dans la section «dans cet article» en dessous du titre sur la page.

### <a name="third-level-heading"></a>Titre de troisième niveau
#### <a name="fourth-level-heading"></a>Titre de la quatrième niveau
##### <a name="fifth-level-heading"></a>En-tête de niveau cinquième
###### <a name="sixth-level-heading"></a>Sixième au niveau de titre

## <a name="text-styling"></a>Style du texte

*Italique* 

**Bold** 

~~Barré~~

## <a name="links"></a>Liens

### <a name="internal-links"></a>Liens internes

Pour lier à un en-tête dans le même fichier de promotions, afficher la source de l’article publié, recherchez l’ID de la tête (par exemple, `id="blockquote"`) et le lier à l’aide de # + id (par exemple, `#blockquote`).

- Exemple: [Blockquotes](#blockquote)

Pour lier à un fichier de promotions dans la même emprunteuses, utilisez [les liens relatifs](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), notamment «.md» à la fin du nom de fichier.

- Exemple: [conseils et astuces](tips-gotchas.md)
- Exemple: [Outils et le programme d’installation pour les collaborateurs](../readme.md)

Pour lier à un en-tête dans un fichier de promotions dans la même emprunteuses, utilisez la liaison relatif + hashtag liaison.

- Exemple: [fichiers de suppression](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Liens externes

Pour créer un lien vers un fichier externe, utilisez l’URL complète du lien.

- Exemple: [référentiels](http://www.github.com)

Si une URL apparaît dans un fichier de promotions, il sera transformé en lien activable.

- Exemple:http://www.github.com

## <a name="lists"></a>Listes

### <a name="ordered-lists"></a>Listes ordonnées

1. Cela 
1. Est
1. Un
1. Commandée
1. List  


#### <a name="ordered-list-with-an-embedded-list"></a>Liste avec une liste intégrée triée

1. Ici
1. est fourni
1. un
1. incorporé
    1. Échecs Scarlett
    1. Prune professeur
1. commandée
1. list


### <a name="unordered-lists"></a>Listes non ordonnées

- Cela
- est
- a
- liste à puces
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Liste non triée avec une liste intégrée

- Cela 
- liste à puces 
- list
    - Mme Lapis-lazuli
    - M. vert
- contient  
- autre
    1. Moutarde soutient le colonel
    1. Livre Mme
- listes


## <a name="horizontal-rule"></a>Règle horizontale

---

## <a name="tables"></a>Tables

Dans presque toutes les instances, utilisez MD mise en forme des tableaux. Tandis que les tables HTML fournissent une plus grande flexibilité nous ne pas les utiliser dans notre contenu. Si vous disposez d’une table HTML dans votre article, nous fusionne pas cet article.

| Tables        | Sont           | Refroidir  |
| ------------- |:-------------:| -----:|
| colonne 3 est      | aligné à droite | $1600 |
| col 2 est      | centré      |   12dollars |
| colonne 1 est la valeur par défaut | aligné à gauche     |    $1 |


## <a name="code"></a>Code

### <a name="generic-codeblock"></a>Codeblock générique

Retrait des espaces de code quatre de codage codeblock générique.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks avec l’identificateur de langue

Utilisez trois backticks (& 96 #; & 96 #; & 96 #;) + un ID de langue à appliquer la couleur propres aux langues codage dans un bloc de code.  Voici la liste complète des [ID de langue référentiels AROMATISES promotions (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C & #9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>Code incorporé

Utilisez backticks & 96 #; pour `inline code`.

## <a name="blockquotes"></a>Blockquotes

> La sécheresse avait dure maintenant 10 millions d’années et le calendrier des lizards terrible avait terminés depuis longtemps. Ici sur l’Équateur, dans le continent qui aurait un jour appelé Afrique, la lutte existence a atteint une nouvelle climax de férocité et le victor n’a pas encore visibles. Dans ces surfaces barren et desséchée, uniquement la petite ou les solutions swift la féroce pu Arabesque ou espère que même de survivre.

## <a name="images"></a>Images

### <a name="static-image"></a>Image statique

![le texte de remplacement](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Image liée

[![alt texte pour l’image liée](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Alertes

### <a name="note"></a>Remarque

> [!NOTE]
> Il s’agit de NOTE

### <a name="warning"></a>Warning

> [!WARNING]
> Ceci est un avertissement

### <a name="tip"></a>Astuce

> [!TIP]
> Il s’agit d’info-bulle

### <a name="important"></a>Important

> [!IMPORTANT]
> C'est important

