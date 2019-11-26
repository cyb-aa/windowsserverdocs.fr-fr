---
title: Migrer du kit de développement logiciel (SDK) du centre d’administration Windows 0,1 vers 1,0
description: Ce guide vous aidera à migrer du kit de développement logiciel (SDK) du centre d’administration Windows vers la version 0,1 à 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: ac9946deb4fa70203a9b05e0386deb4244b8ca55
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2019
ms.locfileid: "71357061"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrer du kit de développement logiciel (SDK) du centre d’administration Windows 0,1 vers 1,0

>S’applique à : version préliminaire du centre d’administration Windows

Ce guide vous aidera à migrer à partir du kit de développement logiciel (SDK) du centre d’administration Windows version 0,1 vers 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. en savoir plus sur les nouveaux contrôles avec l’extension du Guide de développement

Le centre d’administration Windows version 1902 et versions ultérieures inclut l’extension **Guide de développement** , que vous pouvez utiliser pour rechercher des exemples de contrôles (y compris les contrôles récemment disponibles) et des scénarios pour vous aider à créer votre propre extension.  Le Guide de développement remplace l’extension de **outils de développement** des versions antérieures du kit de développement logiciel (SDK).

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Utiliser le Guide de développement dans le centre d’administration Windows

Le Guide de développement est disponible en tant que solution dans le centre d’administration Windows version 1902 et versions ultérieures.  Le Guide de développement est préinstallé, mais il doit être activé via les paramètres.

**Activer le Guide de développement dans le centre d’administration Windows :**

* Ouvrir le centre d’administration Windows (version 1902 et versions ultérieures)
* Cliquez sur l’icône **paramètres** dans le coin supérieur droit de la fenêtre.
* Sélectionnez l’onglet **avancé**
* Sous *clés d’expérimentation*, cliquez sur **Ajouter** .
* Entrez une nouvelle valeur ```msft.sme.shell.devguide``` dans le champ vide qui a été créé à l’étape précédente.
* Cliquez sur **enregistrer et recharger** .

**Ouvrez le Guide de développement dans le centre d’administration Windows :**

* Ouvrir le centre d’administration Windows (version 1902 et versions ultérieures)
* Cliquez sur la liste déroulante en haut à gauche pour afficher tous les types de solution
* Sélectionner la solution de **Guide de développement** 
    * Si vous ne voyez pas la solution dans la liste, vérifiez que vous avez activé le Guide de développement (voir la section ci-dessus) et que vous avez rechargé le centre d’administration Windows.
* Parcourez le contenu du Guide de développement en sélectionnant l’un des onglets.
    * **Palier :** Contient des exemples de code pour les scénarios de *gestion* et de *notification*
    * **Contrôles :** Contient des exemples de chaque contrôle disponible dans le kit de développement logiciel (SDK)
    * **Canaux :** Contient des exemples de fonctions de convertisseur et de formateur disponibles
    * **Styles :** Contient des exemples de styles CSS disponibles dans le kit de développement logiciel (SDK)
    * **MsftSme :** Contient des exemples et des conseils pour les scénarios avancés 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Parcourir le code source du Guide de développement sur GitHub

Vous pouvez parcourir le [code source](https://github.com/Microsoft/windows-admin-center-sdk/) du Guide de développement sur GitHub pour rechercher des exemples de code HTML, CSS et de machine à écrire.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. préparer votre environnement de développement pour le dernier Kit de développement logiciel (SDK)

Installez ou mettez à jour node. js version [10.15.1 LTS ou version ultérieure](https://nodejs.org/en/).

Mettez à jour l’interface de commande du centre d’administration Windows vers la dernière version :

[//]: # "désinstallation de NPM-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Mettez à jour vos dépendances globales avec ces versions :

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. créer un nouveau projet avec le dernier Kit de développement logiciel (SDK)

Utilisez l’interface de commande du centre d’administration Windows pour créer un projet ciblant la version de ```next``` (SDK 1,0) :

[//]: # "WAC Create--Company « contoso Inc »--outil « Manage foo Works »--version expérimentale"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Ensuite, accédez au dossier que vous venez de créer, puis installez les dépendances locales requises en exécutant ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. modifier un projet existant pour utiliser le kit de développement logiciel (SDK) le plus récent

IMPORTANT : effectuez une sauvegarde de votre projet avant de continuer.

Modifiez la ligne suivante dans ```package.json``` pour cibler la version ```next``` (SDK 1,0) :

[//]: # "'@microsoft/windows-admin-center-sdk' : 'expérimental'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Exécutez ensuite ```npm install``` pour mettre à jour les références dans votre projet.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Utilisez l’interface CLI du kit de développement logiciel pour résoudre les problèmes de migration courants

IMPORTANT : effectuez une sauvegarde de votre projet avant de continuer.

À partir du dossier racine de votre projet, exécutez la commande CLI suivante sur votre projet pour corriger automatiquement les problèmes de migration courants :

``` cmd
wac updateSeven --update
```

Cette commande CLI résout automatiquement les problèmes suivants :

* Régénérer ```package-lock.json```
* Mettre à jour les fichiers dans l’environnement de compilation angulaire :
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Utilisez l’interface de commande SDK pour comprendre les problèmes de migration courants

À partir du dossier racine de votre projet, exécutez la commande CLI suivante pour auditer votre projet et recherchez les problèmes de migration courants qui doivent être résolus manuellement :

``` cmd
wac updateSeven --audit
```

Cela permet de trouver des instances des problèmes suivants dans votre projet :

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Remplacez l’utilisation des classes CSS suivantes par ces classes SME :

| Ancienne classe CSS | Nouvelle classe CSS |
| -- | -- |
| . auto-Flex-taille |  . SME-position-Flex-auto |
| . Border-tout |  . SME-Border-incrusté-SM et. SME-Color-Color-base-90 |
| . Border-bas |  . SME-border-bottom-SM et. SME-border-bottom-color-base-90 |
| . Border-horizontal |  . SME-Border-horizontal-SM et. SME-Border-horizontal-Color-base-90 |
| . border-gauche |  . SME-border-left-SM et. SME-border-left-color-base-90 |
| . bordure droite |  . SME-border-right-SM et. SME-border-right-color-base-90 |
| . bordure-haut |  . SME-border-top-SM et. SME-border-top-Color-base-90 |
| . Border-vertical |  . SME-Border-vertical-SM et. SME-Border-vertical-Color-base-90 |
| . Break-Word |  . SME-arrange-WS-Wrap |
| . BTN |  . SME-bouton ou bouton |
| . BTN-principal |  . SME-Button. SME-Button-principal ou. Button. SME-bouton-principal |
| . couleur-sombre |  . SME-couleur-Alt |
| . couleur-clair |  . SME-couleur de base |
| . couleur-clair-gris |  . SME-Color-base-90 |
| . Fixed-Flex-Size |  . SME-position-Flex-None |
| . Flex-disposition |  . SME-arrange-Stack-h ou. SME-arrange-Stack-v |
| . font-gras |  . SME-font-emphasis1 |
| . Mettez en surbrillance |  . SME-arrière-plan-couleur-jaune |
| . horizontal |  . SME-arranger-Stack-h |
| . sans défilement |  . SME-position-Flex-auto |
| . NoWrap |  . SME-arrange-Stack-h ou. SME-arrange-Stack-v |
| . relatif |  . SME-disposition-relative |
| . relative-Centre |  . SME-layout-absolu. SME-position-Center |
| . Reverse |  . SME-arrange-pile-inversé |
| . Stretch-Absolute |  . SME-layout-absolu. SME-position-incrusté-None |
| . Stretch-fixe |  . SME-disposition-Fixed. SME-position-incrusté-None |
| . Stretch-vertical |  . SME-position-Stretch-v |
| . Stretch-largeur |  . SME-position-Stretch-h |
| . vertical |  . SME-arranger-Stack-v |
| . vertical-défilement uniquement |  . SME-organiser-dépassement-masquer-x SME-arrange-Overflow-auto-y |
| . Wrap |  . SME-arrange-wrapstack-h ou. SME-arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Remplacez l’utilisation des composants suivants par ces composants SME :

| Ancien composant | Nouveau composant |
| -- | -- |
| . alerte |  PME-alerte |
| . alerte-danger |  PME-alerte |
| . breadCrumb |  PME-alerte |
| . case à cocher |  SME-champ de forme [type = "checkbox"] |
| . ComboBox |  SME-champ de forme [type = "Select"] |
| . tableau de bord |  SME-layout-content-Padding-arrange-empiler-h |
| . détails-panneau |  SME-propriété-Grid |
| . Details-Panel-Container |  SME-propriété-Grid |
| . détails-onglet |  SME-Property-Grid ou SME-pivot |
| . Details-Wrapper |  SME-propriété-Grid |
| . désactivé |  PME-désactivée |
| . Form-boutons | SME-champ de forme |
| . Form-Control | SME-champ de forme |
| . Form-Controls | SME-champ de forme |
| . Form-Group | SME-champ de forme |
| . Form-Group-étiquette | SME-champ de forme |
| . Form-Input | SME-champ de forme |
| . Form-Stretch | SME-champ de forme |
| . Input-file | SME-champ de forme |
| . NAV-onglets |  SME-pivot |
| . radio |  SME-champ de forme [type = « radio »] |
| . obligatoire-indice | SME-champ de forme |
| . SearchBox |  SME-champ de forme [type = "Rechercher"] |
| . Toggle-Switch |  SME-champ de forme [type = « basculer-commutateur »] |
| . Tool-Container |  SME-layout-content-zone ou SME-layout-content-zone remplie |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Ces classes CSS sont déconseillées et ne sont plus prises en charge :

| Ancienne classe | Déconseillée |
| -- | -- |
| . acceptable | déconseillé |
| . couleur-erreur | déconseillé |
| . Color-info | déconseillé |
| . couleur-réussite | déconseillé |
| . couleur-AVERTISSEMENT | déconseillé |
| . Delete-Button | déconseillé |
| . Details-contenu | déconseillé |
| . erreur-couverture | déconseillé |
| . erreur : message | déconseillé |
| . guidé-bouton-volet | déconseillé |
| . Header-Container | déconseillé |
| . Icon-Win | déconseillé |
| . Indent | déconseillé |
| . non valide | déconseillé |
| . Item-List | déconseillé |
| . modal-Scrollable | déconseillé |
| . plusieurs sections | déconseillé |
| . no-action-bar | déconseillé |
| . Overflow-Margins | déconseillé |
| . Overflow-Tool | déconseillé |
| . Progress-couverture | déconseillé |
| . panneau droit | déconseillé |
| . ROLLUP | déconseillé |
| . ROLLUP-Status | déconseillé |
| . ROLLUP-titre | déconseillé |
| . ROLLUP-value | déconseillé |
| . SearchBox-barre d’action | déconseillé |
| . Size-h-1 | déconseillé |
| . Size-h-2 | déconseillé |
| . Size-h-3 | déconseillé |
| . Size-h-4 | déconseillé |
| . Size-h-Full | déconseillé |
| . Size-h-Half | déconseillé |
| . Size-v-1 | déconseillé |
| . Size-v-2 | déconseillé |
| . Size-v-3 | déconseillé |
| . Size-v-4 | déconseillé |
| . Status-icône | déconseillé |
| . svg-16px | déconseillé |
| . table-Indent | déconseillé |
| . table-SM | déconseillé |
| . fin | déconseillé |
| . Tile | déconseillé |
| . Tile-corps | déconseillé |
| . Tile-contenu | déconseillé |
| . Tile-pied de page | déconseillé |
| . Tile-en-tête | déconseillé |
| . Tile-disposition | déconseillé |
| . Tile-table | déconseillé |
| . ToolBar | déconseillé |
| . barre d’outils | déconseillé |
| . Tool-en-tête | déconseillé |
| . Tool-Box-Header | déconseillé |
| . volet d’outils | déconseillé |
| . barre d’utilisation | déconseillé |
| . utilisation-zone-barre | déconseillé |
| . utilisation-barre-arrière-plan | déconseillé |
| . utilisation-barre-titre | déconseillé |
| . usage-bar-value | déconseillé |
| . utilisation-graphique | déconseillé |
| . utilisation-message | déconseillé |
| . utilisation-message-zone | déconseillé |
| . utilisation-message-title | déconseillé |
| . AVERTISSEMENT | déconseillé |
| . espace blanc | déconseillé |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. comprendre et résoudre les problèmes liés aux objets observables

### <a name="update--rxjs-function-use-for-observable-objects"></a>Mettre à jour ```rxjs``` utilisation des fonctions pour les objets observables

Il s’agit de certains noms de fonctions courants qui ont été modifiés, il peut y en avoir d’autres dans votre projet.

* Mettre à jour ```Observable.empty()``` vers ```empty()```
* Mettre à jour ```Observable.of()``` vers ```of()```
* Mettre à jour ```.switchMap()``` vers ```.pipe(switchMap())```
* Mettre à jour ```.map()``` vers ```.pipe(map())```
* Mettre à jour ```flatMap()``` vers ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Résoudre les problèmes d’exécution avec les fonctions ```.map()``` et ```.filter()``` sur les objets observables

Si le compilateur ne parvient pas à identifier correctement le type d’un objet ```observable```, ```.map()``` et ```.filter()``` fonctions de l’objet ```array``` peuvent être mappées à votre objet, provoquant des erreurs au moment de l’exécution.  Assurez-vous que vos fonctions retournent un objet ```observable``` spécifiant un type de données explicite pour éviter ce problème.

```any``` et aucun type de retour ne peut être à l’origine de ce problème, recherchez le code avec ces modèles :

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. résoudre les autres problèmes courants

Ces techniques vous aideront à résoudre d’autres problèmes courants :

* Exécuter ```ng lint --fix``` pour résoudre les problèmes liés au Lint courant
* Exécuter ```gulp build``` à plusieurs reprises pour résoudre de manière incrémentielle les problèmes que ```gulp build``` peut résoudre automatiquement

## <a name="9-build-and-serve-your-project"></a>9. générez et servez votre projet

Exécutez les commandes suivantes pour générer et servir votre projet avec la version la plus récente (SDK 1,0) :

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. activer le thème sombre dans le centre d’administration Windows

Pour activer le thème sombre dans le centre d’administration Windows version 1902 et versions ultérieures, procédez comme suit :

* Ouvrir le centre d’administration Windows (version 1902 et versions ultérieures)
* Cliquez sur l’icône **paramètres** dans le coin supérieur droit de la fenêtre.
* Sélectionnez l’onglet **avancé**
* Sous *clés d’expérimentation*, cliquez sur **Ajouter** .
* Entrez une nouvelle valeur ```msft.sme.shell.personalization``` dans le champ vide qui a été créé à l’étape précédente.
* Cliquez sur **enregistrer et recharger** .
* Les paramètres disposent désormais d’un nouvel onglet, **personnalisation**.  Sélectionnez cet onglet
* Changer les **couleurs** en **mode Dark (version préliminaire)**
