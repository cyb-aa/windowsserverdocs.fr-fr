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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879560"
---
# <a name="metadata-and-markdown-template"></a>Métadonnées et modèle Markdown

Ce modèle OPS contient des exemples de syntaxe Markdown, ainsi que des conseils sur la définition de métadonnées. Pour obtenir le meilleur parti, vous devez afficher à la fois le [Markdown brut](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) et [vue rendue](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (Le Markdown brut affiche le bloc de métadonnées n’est pas le cas de l’affichage).

Lorsque vous créez un fichier Markdown, vous devez copier ce modèle dans un nouveau fichier, remplir les métadonnées comme indiqué ci-dessous, définir le titre H1 ci-dessus au titre de l’article et supprimer le contenu. Quoi que ce soit dans les limites dans des crochets nécessite votre attention.


## <a name="metadata"></a>Métadonnées 

Le bloc de métadonnées complètes se situe au-dessus. Remarques importantes :

- Vous **doit** ont un espace entre le signe deux-points ( :)) et la valeur d’un élément de métadonnées.
- Signes deux-points dans une valeur (par exemple, un titre) arrêter l’Analyseur de métadonnées. À leur place, utilisez l’encodage HTML pour un signe deux-points de `&#58;` (par exemple, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **titre**: Ce titre s’affiche dans les résultats de recherche. 
- **Auteur**: Le champ author doit contenir le **nom d’utilisateur GitHub** de l’auteur, et non son alias.
- **ms.prod**, **ms.technology**: Utilisez « windows-server-threshold » ms.prod (ou pour w10 si vous utilisez ce modèle pour créer du contenu pour Windows 10). Communiquer avec votre contact CX pour obtenir la valeur ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Markdown de base, GFM et caractères spéciaux

Tous les Markdown de base et GitHub-flavored est pris en charge. Pour plus d’informations, consultez :

- [Syntaxe Markdown de référence](https://daringfireball.net/projects/markdown/syntax)
- [Documentation de Markdown (GFM) GitHub-flavored](https://guides.github.com/features/mastering-markdown)

Markdown utilise des caractères spéciaux tels que \*, \`, et \# pour mettre en forme. Si vous souhaitez inclure l’un de ces caractères dans votre contenu, vous devez effectuer une des deux opérations :

- Placer une barre oblique inverse avant le caractère spécial pour « échapper » (par exemple, \\ \* pour un \*)
- Utilisez le [code d’entité HTML](http://www.ascii.cl/htmlcodes.htm) pour le caractère (par exemple, \& \#42\; pour un &#42;).

## <a name="headings"></a>En-têtes

En-têtes doivent être effectuées à l’aide du style atx, c'est-à-dire, utilisez 1 à 6 caractères de hachage (#) au début de la ligne pour indiquer un titre, correspondant aux niveaux de titres HTML H1 à H6. Exemples d’en-têtes de première et deuxième niveau sont utilisés ci-dessus. 

Il **doit** n'être qu’un seul titre de premier niveau (H1) dans votre rubrique, qui sera affiché comme titre sur la page.  

Titres de deuxième niveau génèrent la table des matières sur la page qui s’affiche dans la section « dans cet article » sous le titre sur la page.

### <a name="third-level-heading"></a>Titre de troisième niveau
#### <a name="fourth-level-heading"></a>Titre de quatrième niveau
##### <a name="fifth-level-heading"></a>Titre de cinquième niveau
###### <a name="sixth-level-heading"></a>Titre de sixième niveau

## <a name="text-styling"></a>Style du texte

*Italique* 

**Gras** 

~~Il est barré~~

## <a name="links"></a>Liens

### <a name="internal-links"></a>Liens internes

Pour lier à un en-tête dans le même fichier Markdown, affichez la source de l’article publié, recherchez l’ID de la tête (par exemple, `id="blockquote"`) et le lier à l’aide de # + id (par exemple, `#blockquote`).

- Exemple : [Blockquotes](#blockquote)

Pour lier à un fichier Markdown dans le même référentiel, utilisez [liens relatifs](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), dont « .md » à la fin du nom de fichier.

- Exemple : [Conseils et pièges](tips-gotchas.md)
- Exemple : [Outils et le programme d’installation pour les contributeurs](../readme.md)

Pour lier à un en-tête dans un fichier Markdown dans le même référentiel, utilisez la liaison relative + la liaison hashtag.

- Exemple : [Suppression de fichiers](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Liens externes

Pour créer un lien vers un fichier externe, utilisez l’URL complète comme lien.

- Exemple : [GitHub](http://www.github.com)

Si une URL apparaît dans un fichier Markdown, elle est transformée en lien interactif.

- Exemple : http://www.github.com

## <a name="lists"></a>Listes

### <a name="ordered-lists"></a>Listes triées

1. Cette 
1. est
1. Un
1. Commandée
1. List  


#### <a name="ordered-list-with-an-embedded-list"></a>Liste triée avec une liste incorporée

1. Ici
1. est fourni
1. Un
1. Embedded
    1. Mademoiselle rose
    1. Professeur violet
1. Commandée
1. list


### <a name="unordered-lists"></a>Listes non triées

- Cette
- est
- a
- liste à puces
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Liste non triée avec une liste incorporée

- Cette 
- liste à puces 
- list
    - Madame pervenche
    - Révérend olive
- contient  
- Autre
    1. Colonel moutarde
    1. Madame LeBlanc
- Listes


## <a name="horizontal-rule"></a>Règle horizontale

---

## <a name="tables"></a>Tables

Dans presque chaque instance, utilisez MD mise en forme pour les tables. Tandis que les tableaux HTML fournissent davantage de flexibilité nous ne sont pas les utiliser dans notre contenu. Si vous avez une table HTML dans votre article, nous allons fusionner pas cet article.

| Tables        | sont           | Froid  |
| ------------- |:-------------:| -----:|
| 3e colonne est      | right-aligned | $1600 |
| 2e colonne est      | centré      |   12 dollars |
| col 1 est la valeur par défaut | left-aligned     |    $1 |


## <a name="code"></a>Code

### <a name="generic-codeblock"></a>Codeblock générique

Retrait des espaces de code quatre pour le codage codeblock générique.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks avec l’identificateur de langue

Utilisez trois accents graves (&#96;&#96;&#96;) + un ID de langue pour appliquer des couleurs spécifique au langage de codage pour un bloc de code.  Voici la liste complète des [ID de langue GitHub Flavored Markdown (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C&#9839;

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

### <a name="inline-code"></a>Code inline

Utilisez les accents graves (&#96;) pour `inline code`.

## <a name="blockquotes"></a>Citations

> La sécheresse durait maintenant depuis dix millions ans, et le règne des terribles lézards avait depuis longtemps pris fin. Ici à l’Équateur, sur le continent serait un jour être connue sous le nom en Afrique, la bataille existence avait atteint un nouveau sommet férocité, et le vainqueur n’était pas encore dans la vue. Dans ce land territoire aride et Désolé, uniquement le petit ou le swift l’acharnée pourrait culturelle ou espérer survivre.

## <a name="images"></a>Images

### <a name="static-image"></a>Image statique

![Il s’agit du texte de remplacement](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Image liée

[![texte de remplacement de l’image liée](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Alertes

### <a name="note"></a>Remarque

> [!NOTE]
> Voici une remarque

### <a name="warning"></a>Warning

> [!WARNING]
> Voici un avertissement

### <a name="tip"></a>Conseil

> [!TIP]
> Il s’agit d’info-bulle

### <a name="important"></a>Important

> [!IMPORTANT]
> C'est important

