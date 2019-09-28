---
title: Désactiver les Fichiers hors connexion sur des dossiers redirigés individuels
description: Comment désactiver la mise en cache Fichiers hors connexion sur des dossiers individuels redirigés vers des partages réseau à l’aide de la redirection de dossiers.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394401"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Désactiver les Fichiers hors connexion sur des dossiers redirigés individuels

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canal semi-annuel)

Cette rubrique explique comment désactiver la mise en cache Fichiers hors connexion sur des dossiers individuels redirigés vers des partages réseau à l’aide de la redirection de dossiers. Cela permet de spécifier les dossiers à exclure de la mise en cache locale, ce qui réduit la taille du cache Fichiers hors connexion et le temps nécessaire à la synchronisation des Fichiers hors connexion.

>[!NOTE]
>Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez la page [concepts de base de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prérequis

Pour désactiver Fichiers hors connexion la mise en cache de dossiers redirigés spécifiques, votre environnement doit répondre aux conditions préalables suivantes.

- Un domaine Active Directory Domain Services (AD DS), avec les ordinateurs clients joints au domaine. Il n’existe aucune exigence de niveau fonctionnel de forêt ou de domaine ou de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows (canal semi-annuel).
- Un ordinateur sur lequel la gestion des stratégie de groupe est installée.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Désactivation d’Fichiers hors connexion sur des dossiers redirigés individuels

Pour désactiver la mise en cache Fichiers hors connexion de dossiers redirigés spécifiques, utilisez stratégie de groupe pour activer le paramètre de stratégie **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles hors connexion** pour l’objet de stratégie de groupe (GPO) approprié. Si vous configurez ce paramètre de stratégie sur **désactivé** ou **n’est pas configuré** , tous les dossiers redirigés sont disponibles hors connexion.

>[!NOTE]
>Seuls les administrateurs de domaine, les administrateurs d’entreprise et les membres du groupe stratégie de groupe Propriétaires créateurs peuvent créer des objets de stratégie de groupe.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Pour désactiver les Fichiers hors connexion sur des dossiers redirigés spécifiques

1. Ouvrez la **gestion des stratégie de groupe**.
2. Pour éventuellement créer un nouvel objet de stratégie de groupe qui spécifie les utilisateurs qui ne doivent pas avoir de dossiers redirigés disponibles hors connexion, cliquez avec le bouton droit sur le domaine ou l’unité d’organisation approprié, puis sélectionnez **créer un objet de stratégie de groupe dans ce domaine, et le lier ici** .
3. Dans l’arborescence de la console, cliquez avec le bouton droit sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres de redirection de dossiers, puis sélectionnez **modifier**. Le Éditeur de gestion des stratégies de groupe s’affiche.
4. Dans l’arborescence de la console, sous **Configuration utilisateur**, développez successivement **stratégies**, **modèles d’administration**, **système**et **redirection de dossiers**.
5. Cliquez avec le bouton droit sur **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles en mode hors connexion** , puis sélectionnez **modifier**. La fenêtre **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles hors connexion** s’affiche.
6. Sélectionnez **Activé**. Dans le volet **options** , sélectionnez les dossiers qui ne doivent pas être rendus disponibles hors connexion en activant les cases à cocher appropriées. Sélectionnez **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

L’applet de commande ou les applets de commande Windows PowerShell suivantes effectuent la même fonction que la procédure décrite dans la section [Désactivation d’fichiers hors connexion sur des dossiers redirigés individuels](#disabling-offline-files-on-individual-redirected-folders). Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

Cet exemple crée un objet de stratégie de groupe nommé *fichiers hors connexion paramètres* dans l’unité d’organisation *MyOu* dans le domaine *contoso.com* (le nom unique LDAP est « ou = MyOu, DC = contoso, DC = com »). Il désactive ensuite Fichiers hors connexion pour le dossier de vidéos redirigées.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consultez le tableau suivant pour obtenir la liste des noms de clé de Registre (GUID de dossier) à utiliser pour chaque dossier redirigé.

|Dossier Redirigé|Nom de la clé de Registre (GUID du dossier)|
|---|---|
|AppData (itinérance)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Bureau|B4BFCC3A-DB2C-424C-B029-7FE99A87C641|
|Menu Démarrer|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documents|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Images|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Musique|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vidéos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favoris|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contacts|{56784854-C6CB-462B-8169-88E350ACB882}|
|Téléchargements|{374DE290-123F-4565-9164-39C4925E467B}|
|Liens|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Recherches|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Parties enregistrées|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Plus d’informations

- [Vue d’ensemble de la redirection de dossiers, des Fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)