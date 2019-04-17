---
title: Migrer à partir du Kit de développement logiciel Windows Admin Center 0.1 à 1.0
description: Ce guide vous aidera à migrer à partir du SDK Windows Admin Center version 0.1 à 1.0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116585"
---
# Migrer à partir du Kit de développement logiciel Windows Admin Center 0.1 à 1.0

>S’applique à: Windows Admin Center Preview

Ce guide vous aidera à migrer à partir du SDK Windows Admin Center version 0.1 à 1.0.  

## 1. en savoir plus sur les nouveaux contrôles avec l’extension du Guide de développement

Windows Admin Center version 1902 et versions ultérieures inclut l’extension de **Guide de développement** , vous pouvez utiliser pour rechercher des exemples des contrôles (y compris les contrôles qui vient d’être disponibles) et des scénarios pour vous aider à créer votre propre extension.  Guide de développement remplace l’extension **d’Outils de développement** à partir de versions antérieures du SDK.

### Utilisez le Guide de développement de Windows Admin Center

Guide de développement est disponible en tant que solution dans la version de Windows Admin Center 1902 et versions ultérieures.  Guide de développement est préinstallée, mais il doit être activé via les paramètres.

**Activer le Guide de développement de Windows Admin Center:**

* Ouvrir Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur l’icône **paramètres** dans le coin supérieur droit de la fenêtre
* Sélectionnez l’onglet **Avancé**
* Sous les *Clés de l’expérience*, cliquez sur **Ajouter**
* Entrez une nouvelle valeur ```msft.sme.shell.devguide``` dans le champ vide qui a été créé par l’étape précédente
* Cliquez sur **Enregistrer et recharger**

**Ouvrez le Guide de développement de Windows Admin Center:**

* Ouvrir Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur le menu déroulant dans le coin supérieur gauche pour afficher tous les types de solution
* Sélectionnez la solution **Guide de développement** 
    * Si vous ne voyez pas la solution répertoriée, assurez-vous que vous avez activé le guide de développement (voir la section ci-dessus) et avoir rechargé Windows Admin Center.
* Parcourir le contenu du Guide de développement en sélectionnant l’une des onglets
    * **D’accueil:** Contient des exemples de code pour les scénarios de *Notification* et de *Gérer en tant que*
    * **Contrôles:** Contient des exemples de chaque contrôle disponible dans le Kit de développement
    * **Canaux:** Contient des exemples de fonctions de convertisseur et formateur disponibles
    * **Styles:** Contient des exemples de styles CSS disponibles dans le Kit de développement
    * **MsftSme:** Contient des exemples et des conseils pour les scénarios avancés 

### Parcourir le code source du Guide de développement sur GitHub

Vous pouvez parcourir le [code source](https://github.com/Microsoft/windows-admin-center-sdk/) du Guide de développement sur GitHub pour trouver des exemples de code HTML, CSS et TypeScript exemple.

## 2. préparer votre environnement de développement pour la dernière version du SDK

Installer ou mettre à jour de la version node.js [10.15.1 LTS ou une version ultérieure](https://nodejs.org/en/).

Mettre à jour l’interface CLI de Windows Admin Center vers la dernière version:

[//]: # "npm désinstaller windows-admin-center-cli@next -g"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Mettre à jour vos dépendances globales vers ces versions:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3. créer un nouveau projet avec la dernière version du SDK

L’interface CLI de Windows Admin Center permet de créer un nouveau projet qui cible la ```next``` version (1.0 SDK):

[//]: # "créer de wac--«Contoso Inc.»--outil «gérer Foo Works'--version expérimentale de société"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Ensuite, modifiez le répertoire dans le dossier venez de créer, puis installez des dépendances locales requis en exécutant ```npm install ```.

## 4. modifier un projet existant pour utiliser la dernière version du SDK

IMPORTANT: Effectuer une sauvegarde de votre projet avant de continuer.

Modifiez la ligne suivante dans ```package.json``` à cibler la ```next``` version (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': «expérimental»"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Ensuite, exécutez ```npm install``` pour mettre à jour les références tout au long de votre projet.

## 5. utiliser la CLI SDK pour résoudre les problèmes de migration courants

IMPORTANT: Effectuer une sauvegarde de votre projet avant de continuer.

À partir du dossier racine de votre projet, exécutez la commande suivante de la CLI sur votre projet pour résoudre les problèmes de migration courants automatiquement:

``` cmd
wac updateSeven --update
```

Cette commande CLI corrige automatiquement les problèmes suivants:

* Régénérer ```package-lock.json```
* Fichiers de mise à jour dans l’environnement de compilation angulaire:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6. utiliser la CLI SDK pour comprendre les problèmes de migration courants

À partir du dossier racine de votre projet, exécutez la commande suivante de la CLI pour votre projet d’audit et de rechercher des problèmes courants de migration qui doivent être traités manuellement:

``` cmd
wac updateSeven --audit
```

Cela trouve instances des problèmes suivants dans votre projet:

### Avec ces classes PME, remplacez l’utilisation des classes CSS suivantes:

| Classe CSS ancien | Classe CSS |
| -- | -- |
| taille .auto-flexible |  .SME-position-flexible-automatique |
| .Border-all |  .SME-bordure-incrusté-sm et .sme-bordure-couleur-base de-90 |
| .Border en bas |  .SME-bordure-bas-sm et .sme-border-bottom-color-base-90 |
| .Border-horizontal |  .SME-bordure-horizontal-sm et .sme-border-horizontal-color-base-90 |
| .Border à gauche |  .SME-bordure-gauche-sm et .sme-border-left-color-base-90 |
| .Border à droite |  .SME-bordure-droite-sm et .sme-border-right-color-base-90 |
| .Border-haut |  .SME-Bordure-haut-sm et .sme-border-top-color-base-90 |
| .Border-vertical |  .SME-bordure-vertical-sm et .sme-border-vertical-color-base-90 |
| .Break-word |  .SME-organiser-ws-wrap |
| .BTN |  .SME-bouton bouton OR |
| .BTN-principal |  .SME-button.sme-bouton-principal ou.button.sme-bouton-principal |
| .Color-sombre |  .SME-couleur-alt |
| lumière .color |  base de couleur .sme |
| .Color-gris clair |  .SME-couleur-base de-90 |
| taille .fixed-flexible |  .SME-position-flexible-aucun |
| disposition de .flex |  .SME-organiser-pile-h ou .sme-organiser-pile-v |
| .font-gras |  .SME-police-emphasis1 |
| .Highlight |  .SME-arrière-plan-jaune |
| .horizontal |  .SME-organiser-pile-h |
| C’est à défilement |  .SME-position-flexible-automatique |
| .NoWrap |  .SME-organiser-pile-h ou .sme-organiser-pile-v |
| .relative |  disposition .sme-relative |
| .relative-center |  disposition .sme-absolus .sme-position-center |
| .Reverse |  .SME organiser-pile-inversées |
| .Stretch absolus |  .SME-disposition-absolue .sme-position-incrusté-aucun |
| .Stretch fixe |  .SME disposition-fixe .sme-position-incrusté-aucun |
| .Stretch-vertical |  .SME-position-stretch-v |
| .Stretch chasse |  .SME-position-stretch-h |
| .vertical |  .SME-organiser-pile-v |
| défilement .vertical uniquement |  .SME-organiser-dépassement-masquer-x PME-organiser-dépassement-auto-y |
| .Wrap |  .SME-organiser-wrapstack-h ou .sme-organiser-wrapstack-v |

### Avec ces composants PME, remplacez l’utilisation des composants suivants:

| Ancien composant | Nouveau composant |
| -- | -- |
| .Alert |  alerte-PME |
| .Alert-danger |  alerte-PME |
| .breadCrumb |  alerte-PME |
| .CheckBox |  champ de formulaire PME [type = «checkbox»] |
| .ComboBox |  champ de formulaire PME [type = «sélectionner»] |
| .Dashboard |  SME disposition-contenu-zone-complétées par des PME-organiser-pile-h |
| .Details-panneau |  grille de propriété PME |
| conteneur de panneaux .details |  grille de propriété PME |
| .Details-tab |  grille de propriétés PME ou PME-pivot |
| .Details-wrapper |  grille de propriété PME |
| .Disabled |  PME est désactivée |
| .les-boutons | champ de formulaire PME |
| contrôle .les | champ de formulaire PME |
| .les-contrôles | champ de formulaire PME |
| .les-group | champ de formulaire PME |
| nom du groupe .les | champ de formulaire PME |
| .les-entrée | champ de formulaire PME |
| .les-stretch | champ de formulaire PME |
| -le fichier .input | champ de formulaire PME |
| .NAV-onglets |  PME-pivot |
| .radio |  champ de formulaire PME [type = «radio»] |
| .Required-indice | champ de formulaire PME |
| .Searchbox |  champ de formulaire PME [type = «Rechercher»] |
| .Toggle-commutateur |  champ de formulaire PME [type = «bascule»] |
| conteneur-.tool |  PME-disposition-contenu-zone ou PME-disposition-contenu-zone-remplis |

### Ces classes CSS sont déconseillés et ne sont plus prises en charge:

| Ancienne classe | Obsolète |
| -- | -- |
| .acceptable | (déconseillée) |
| .Color-erreur | (déconseillée) |
| .Color-informations | (déconseillée) |
| .Color-réussite | (déconseillée) |
| .Color-avertissement | (déconseillée) |
| boutons .Supprimer & nbsp | (déconseillée) |
| .Details-contenu | (déconseillée) |
| .Error-garde | (déconseillée) |
| .error-message | (déconseillée) |
| bouton du volet de .guided | (déconseillée) |
| conteneur-.header | (déconseillée) |
| win-.icon | (déconseillée) |
| .Indent | (déconseillée) |
| .Invalid | (déconseillée) |
| .Item-list | (déconseillée) |
| avec défilement .modal | (déconseillée) |
| . section multiples | (déconseillée) |
| C’est barre d’actions | (déconseillée) |
| .Overflow-marges | (déconseillée) |
| outil de .overflow | (déconseillée) |
| .Progress-garde | (déconseillée) |
| .Right-panneau | (déconseillée) |
| .ROLLUP | (déconseillée) |
| état .rollup | (déconseillée) |
| .ROLLUP-titre | (déconseillée) |
| valeur .rollup | (déconseillée) |
| barre d’actions .searchbox | (déconseillée) |
| .Size-h-1 | (déconseillée) |
| .Size-h-2 | (déconseillée) |
| .Size-h-3 | (déconseillée) |
| .Size-h-4 | (déconseillée) |
| .Size plein h | (déconseillée) |
| .Size h demi | (déconseillée) |
| .Size-v-1 | (déconseillée) |
| .Size-v-2 | (déconseillée) |
| .Size-v-3 | (déconseillée) |
| .Size-v-4 | (déconseillée) |
| icône .status | (déconseillée) |
| .SVG-16 px | (déconseillée) |
| mise en retrait de .table | (déconseillée) |
| .table-sm | (déconseillée) |
| .Thin | (déconseillée) |
| .Tile | (déconseillée) |
| .Tile-body | (déconseillée) |
| .Tile-contenu | (déconseillée) |
| .Tile-pied de page | (déconseillée) |
| en-tête .tile | (déconseillée) |
| disposition de .tile | (déconseillée) |
| .Tile-tableau | (déconseillée) |
| .ToolBar | (déconseillée) |
| barre de .tool | (déconseillée) |
| en-tête .tool | (déconseillée) |
| zone de l’en-tête .tool | (déconseillée) |
| volet .tool | (déconseillée) |
| barre de .usage | (déconseillée) |
| zone de barre de .usage | (déconseillée) |
| .usage-barre-en arrière-plan | (déconseillée) |
| .usage-barre-titre | (déconseillée) |
| .usage-barre-value | (déconseillée) |
| .usage-graphique | (déconseillée) |
| .usage-message | (déconseillée) |
| zone de message .usage | (déconseillée) |
| titre du message .usage | (déconseillée) |
| .Warning | (déconseillée) |
| espace de .white | (déconseillée) |

## 7. comprendre et résoudre les problèmes avec les objets observables

### Mise à jour ```rxjs``` fonction pour les objets observables

Voici certains noms de fonction courantes qui ont été modifiées, il existe peut-être d’autres personnes dans votre projet.

* Mise à jour ```Observable.empty()``` à ```empty()```
* Mise à jour ```Observable.of()``` à ```of()```
* Mise à jour ```.switchMap()``` à ```.pipe(switchMap())```
* Mise à jour ```.map()``` à ```.pipe(map())```
* Mise à jour ```flatMap()``` à ```mergeMap()```


### Résoudre les problèmes d’exécution avec ```.map()``` et ```.filter()``` fonctions sur les objets observables

Si le compilateur ne peut pas identifier correctement un ```observable``` type d’objet, ```.map()``` et ```.filter()``` fonctionne à partir de la ```array``` objet peut être mappé à la place à votre objet, à l’origine des erreurs lors de l’exécution.  Assurez-vous que vos fonctions retournent une ```observable``` objet en spécifiant le type de données explicite pour contourner ce problème.

```any``` et aucun type de retour peuvent provoquer ce problème, recherchez le code avec ces modèles:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8. résoudre d’autres problèmes courants

Ces techniques permettent de résoudre d’autres problèmes courants:

* Exécutez ```ng lint --fix``` à résoudre les problèmes courants lint
* Exécutez ```gulp build``` à plusieurs reprises de manière incrémentielle corriger les problèmes qui ```gulp build``` peut résoudre automatiquement

## 9. Créez et votre projet

Exécutez les commandes suivantes pour générer et servir à votre projet avec la dernière version (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## 10. activer un thème foncé dans Windows Admin Center

Pour activer un thème foncé dans Windows Admin Center version 1902 et versions ultérieures, procédez comme suit:

* Ouvrir Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur l’icône **paramètres** dans le coin supérieur droit de la fenêtre
* Sélectionnez l’onglet **Avancé**
* Sous les *Clés de l’expérience*, cliquez sur **Ajouter**
* Entrez une nouvelle valeur ```msft.sme.shell.personalization``` dans le champ vide qui a été créé par l’étape précédente
* Cliquez sur **Enregistrer et recharger**
* Paramètres possède alors un nouvel onglet, **personnalisation**.  Sélectionnez cet onglet
* Modifier les **couleurs** en mode **sombre (version d’évaluation)**
