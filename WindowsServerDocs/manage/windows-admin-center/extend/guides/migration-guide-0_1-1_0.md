---
title: Migrer à partir du Kit de développement logiciel Windows Admin Center 0,1 à 1,0
description: Ce guide vous aidera à migrer à partir de Windows Admin Center SDK version 0.1 à 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886470"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrer à partir du Kit de développement logiciel Windows Admin Center 0,1 à 1,0

>S'applique à : Windows Admin Center Preview

Ce guide vous aidera à migrer à partir de Windows Admin Center SDK version 0.1 à 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. En savoir plus sur les nouveaux contrôles avec l’extension du Guide de développement

Windows Admin Center version 1902 et versions ultérieure inclut la **Dev Guide** extension, qui vous permet de trouver des exemples de contrôles (y compris les contrôles qui vient d’être disponibles) et des scénarios pour vous aider à créer votre propre extension.  Guide de développement remplace le **outils de développement** extension à partir de versions antérieures du Kit de développement.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Utilisez le Guide de développement de Windows Admin Center

Guide de développement est disponible en tant que solution dans la version de Windows Admin Center 1902 et versions ultérieures.  Guide de développement est déjà installé, mais il doit être activé par le biais de paramètres.

**Activer le Guide de développement dans Windows Admin Center :**

* Ouvrez Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur le **paramètres** icône dans l’angle supérieur droit de la fenêtre
* Sélectionnez le **avancé** onglet
* Sous *expérience clés*, cliquez sur **ajouter**
* Entrez une nouvelle valeur ```msft.sme.shell.devguide``` dans le champ vide qui a été créé par l’étape précédente
* Cliquez sur **enregistrer et recharger**

**Ouvrir le Guide de développement de Windows Admin Center :**

* Ouvrez Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur la liste déroulante dans le coin supérieur gauche pour afficher tous les types de solution
* Sélectionnez le **Dev Guide** solution 
    * Si vous ne voyez pas la solution répertoriée, vérifiez que vous avez activé le guide de développement (consultez la section ci-dessus) et avez rechargé Windows Admin Center.
* Parcourir le contenu du Guide de développement en sélectionnant un des onglets
    * **Destination :** Contient des exemples de code pour *gérer en tant que* et *Notification* scénarios
    * **Contrôles :** Contient des exemples de chaque contrôle disponibles dans le Kit de développement
    * **Canaux :** Contient des exemples de fonctions de convertisseur et formateur disponibles
    * **Styles :** Contient des exemples de styles CSS disponibles dans le Kit de développement
    * **MsftSme :** Contient des exemples et des conseils pour les scénarios avancés 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Parcourir le code source du Guide de développement de GitHub

Vous pouvez parcourir le [code source](https://github.com/Microsoft/windows-admin-center-sdk/) du Guide de développement de GitHub pour obtenir des exemples de code HTML, CSS et TypeScript exemple.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Préparer votre environnement de développement pour la dernière version du SDK

Installer ou mettre à jour de la version de node.js [10.15.1 LTS ou version ultérieure](https://nodejs.org/en/).

Mettre à jour l’interface CLI de Windows Admin Center vers la dernière version :

[//]: # "npm désinstaller -g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Mettre à jour vos dépendances globales vers ces versions :

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Créer un nouveau projet avec la dernière version du SDK

Utiliser l’interface Windows Admin Center CLI pour créer un nouveau projet qui cible le ```next``` version (1.0 SDK) :

[//]: # "wac create--entreprise « Contoso Inc'--outil « Gérer Foo Works »--version expérimentale"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Ensuite, changez de répertoire dans le dossier venez de créer, puis installez les dépendances requises locales en exécutant ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modifier un projet existant pour utiliser la dernière version du SDK

IMPORTANT : Effectuez une sauvegarde de votre projet avant de continuer.

Modifier la ligne suivante dans ```package.json``` à cibler le ```next``` version (1.0 SDK) :

[//]: # "«@microsoft/windows-admin-center-sdk» : « expérimental »"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Puis exécutez ```npm install``` pour mettre à jour les références dans votre projet.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Utilisez la CLI de SDK pour résoudre les problèmes courants de migration

IMPORTANT : Effectuez une sauvegarde de votre projet avant de continuer.

À partir du dossier racine de votre projet, exécutez la commande CLI suivante sur votre projet pour corriger automatiquement les problèmes courants de migration :

``` cmd
wac updateSeven --update
```

Cette commande CLI résout automatiquement les problèmes suivants :

* Régénérer la clé ```package-lock.json```
* Fichiers de mises à jour dans l’environnement de compilation angular :
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Utilisez le SDK CLI pour comprendre les problèmes courants de migration

À partir du dossier racine de votre projet, exécutez la commande CLI suivante d’auditer votre projet et de rechercher des problèmes courants de migration qui devront être corrigés manuellement :

``` cmd
wac updateSeven --audit
```

Cela permet de rechercher les instances des problèmes suivants dans votre projet :

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Avec ces classes de PME, remplacez l’utilisation des classes CSS suivantes :

| Ancienne classe CSS | Nouvelle classe CSS |
| -- | -- |
| .auto-flex-size |  .sme-position-flex-auto |
| .border-all |  .SME-border-incrustation-sm et .sme-border-color-base-90 |
| .border-bottom |  .SME-border-bas-sm et .sme-border-bottom-color-base-90 |
| .border-horizontal |  .SME-border-horizontale-sm et .sme-border-horizontal-color-base-90 |
| .border-left |  .SME-border-gauche-sm et .sme-border-left-color-base-90 |
| .border-right |  .SME-border-droite-sm et .sme-border-right-color-base-90 |
| .border-top |  .SME-border-top-sm et .sme-border-top-color-base-90 |
| .border-vertical |  .SME-border-vertical-sm et .sme-border-vertical-color-base-90 |
| .break-word |  .sme-arrange-ws-wrap |
| .btn |  bouton de .sme bouton d’OR |
| .BTN primaire |  .SME-button.sme-bouton-primary OR.button.sme-bouton-principal |
| .color-dark |  .sme-color-alt |
| .color-light |  .sme-color-base |
| .Color-gris clair |  .sme-color-base-90 |
| .fixed-flex-size |  .sme-position-flex-none |
| .flex-layout |  .SME-organiser-stack-h ou .sme-organiser-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .highlight |  .sme-background-color-yellow |
| .horizontal |  .sme-arrange-stack-h |
| C’est le défilement |  .sme-position-flex-auto |
| .nowrap |  .SME-organiser-stack-h ou .sme-organiser-stack-v |
| .relative |  .sme-layout-relative |
| .relative-centre |  .SME-disposition-absolue .sme-position-centre |
| .reverse |  .SME réorganiser-stack-inversées |
| .stretch-absolute |  .SME disposition-absolu .sme position-incrustation-aucun |
| .stretch-fixed |  .SME disposition fixe .sme position-incrustation-aucun |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .sme-position-stretch-h |
| .vertical |  .sme-arrange-stack-v |
| défilement .vertical uniquement |  .SME-organiser-dépassement de capacité-masquer-x sme-organiser-overflow-auto-y |
| .wrap |  .SME-organiser-wrapstack-h ou .sme-organiser-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Avec ces composants de PME, remplacez l’utilisation des composants suivants :

| Ancien composant | Nouveau composant |
| -- | -- |
| .alert |  alerte de PME |
| .alert-danger |  alerte de PME |
| .breadCrumb |  alerte de PME |
| .checkbox |  sme-form-field[type="checkbox"] |
| .combobox |  sme-form-field[type="select"] |
| .dashboard |  PME-disposition-contenu-zone-complétée sme-organiser-stack-h |
| .Details-Panneau de configuration |  sme-property-grid |
| .details-panel-container |  sme-property-grid |
| .details-tab |  grille de propriétés de PME OR sme-tableau croisé dynamique |
| .details-wrapper |  sme-property-grid |
| .disabled |  PME-désactivé |
| boutons de .les | champ de formulaire de PME |
| contrôle de .les | champ de formulaire de PME |
| .les-contrôles | champ de formulaire de PME |
| .form-group | champ de formulaire de PME |
| .form-group-label | champ de formulaire de PME |
| .les-input | champ de formulaire de PME |
| .les-stretch | champ de formulaire de PME |
| .input-file | champ de formulaire de PME |
| .nav-tabs |  sme-pivot |
| .radio |  sme-form-field[type="radio"] |
| .required-clue | champ de formulaire de PME |
| .searchbox |  sme-form-field[type="search"] |
| .toggle-switch |  sme-form-field[type="toggle-switch"] |
| .tool-container |  PME-disposition-contenu-zone ou sme-disposition-contenu-zone-complétée |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Ces classes CSS sont déconseillés et ne sont plus prises en charge :

| Ancienne classe | Déconseillée |
| -- | -- |
| .acceptable | (déconseillée) |
| .color-error | (déconseillée) |
| .color-info | (déconseillée) |
| .Color-réussite | (déconseillée) |
| .color-warning | (déconseillée) |
| .delete-button | (déconseillée) |
| .details-content | (déconseillée) |
| .error-cover | (déconseillée) |
| .error-message | (déconseillée) |
| .guided-pane-button | (déconseillée) |
| .header-container | (déconseillée) |
| .icon-win | (déconseillée) |
| .indent | (déconseillée) |
| .invalid | (déconseillée) |
| .item-list | (déconseillée) |
| .Modal avec défilement | (déconseillée) |
| .multi-section | (déconseillée) |
| .no-action-bar | (déconseillée) |
| .overflow-margins | (déconseillée) |
| .overflow-tool | (déconseillée) |
| .progress-cover | (déconseillée) |
| .right-panel | (déconseillée) |
| .rollup | (déconseillée) |
| .ROLLUP-état | (déconseillée) |
| .rollup-title | (déconseillée) |
| .rollup-value | (déconseillée) |
| .searchbox-action-bar | (déconseillée) |
| .size-h-1 | (déconseillée) |
| .size-h-2 | (déconseillée) |
| .size-h-3 | (déconseillée) |
| .size-h-4 | (déconseillée) |
| .size-h-full | (déconseillée) |
| .size-h-half | (déconseillée) |
| .size-v-1 | (déconseillée) |
| .size-v-2 | (déconseillée) |
| .size-v-3 | (déconseillée) |
| .size-v-4 | (déconseillée) |
| .status-icon | (déconseillée) |
| .svg-16px | (déconseillée) |
| mise en retrait .table | (déconseillée) |
| .table-sm | (déconseillée) |
| .thin | (déconseillée) |
| .tile | (déconseillée) |
| .tile-body | (déconseillée) |
| .tile-content | (déconseillée) |
| .tile-footer | (déconseillée) |
| .tile-header | (déconseillée) |
| .tile-layout | (déconseillée) |
| .tile-table | (déconseillée) |
| .toolbar | (déconseillée) |
| .tool-bar | (déconseillée) |
| .tool-header | (déconseillée) |
| .tool-header-box | (déconseillée) |
| volet .tool | (déconseillée) |
| barre de .usage | (déconseillée) |
| zone de la barre de .usage | (déconseillée) |
| arrière-plan de barre .usage | (déconseillée) |
| .usage barre de titre | (déconseillée) |
| valeur .usage-barre | (déconseillée) |
| .usage-graphique | (déconseillée) |
| .usage-message | (déconseillée) |
| .usage-message-area | (déconseillée) |
| .usage-message-title | (déconseillée) |
| .warning | (déconseillée) |
| .white-space | (déconseillée) |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Comprendre et résoudre les problèmes avec les objets observables

### <a name="update--rxjs-function-use-for-observable-objects"></a>Mise à jour ```rxjs``` fonction utilisation pour les objets observables

Il s’agit de certains noms de fonction commune qui ont été modifiés, il peut y avoir d’autres utilisateurs dans votre projet.

* Mise à jour ```Observable.empty()``` à ```empty()```
* Mise à jour ```Observable.of()``` à ```of()```
* Mise à jour ```.switchMap()``` à ```.pipe(switchMap())```
* Mise à jour ```.map()``` à ```.pipe(map())```
* Mise à jour ```flatMap()``` à ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Résoudre les problèmes de runtime ```.map()``` et ```.filter()``` fonctions sur les objets observables

Si le compilateur ne peut pas identifier correctement un ```observable``` type d’objet, ```.map()``` et ```.filter()``` fonctions à partir de la ```array``` objet peut être mappé à la place à votre objet, et provoquent des erreurs lors de l’exécution.  Assurez-vous que vos fonctions retournent un ```observable``` objet spécifiant le type de données explicite pour éviter ce problème.

```any``` et aucun type de retour peut entraîner ce problème, recherchez le code avec ces modèles :

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Résoudre les autres problèmes courants

Ces techniques aideront à résoudre d’autres problèmes courants :

* Exécutez ```ng lint --fix``` pour résoudre les problèmes courants de lint
* Exécutez ```gulp build``` à plusieurs reprises pour résoudre de façon incrémentielle les problèmes qui ```gulp build``` peut résoudre automatiquement

## <a name="9-build-and-serve-your-project"></a>9. Générer et répondre à votre projet

Exécutez les commandes suivantes pour générer et de répondre à votre projet avec la dernière version (1.0 SDK) :

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Activer le thème sombre dans Windows Admin Center

Pour activer le thème sombre dans Windows Admin Center version 1902 et versions ultérieures, procédez comme suit :

* Ouvrez Windows Admin Center (version 1902 et versions ultérieure)
* Cliquez sur le **paramètres** icône dans l’angle supérieur droit de la fenêtre
* Sélectionnez le **avancé** onglet
* Sous *expérience clés*, cliquez sur **ajouter**
* Entrez une nouvelle valeur ```msft.sme.shell.personalization``` dans le champ vide qui a été créé par l’étape précédente
* Cliquez sur **enregistrer et recharger**
* Paramètres seront ont désormais un nouvel onglet, **personnalisation**.  Sélectionnez cet onglet
* Modification **couleurs** à **mode foncé (version préliminaire)**
