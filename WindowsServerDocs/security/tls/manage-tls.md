---
title: Gérer le protocole TLS (Transport Layer Security)
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: f691775d5ab24de8b23df048c13ec3d7c572833f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870286"
---
# <a name="manage-transport-layer-security-tls"></a>Gérer le protocole TLS (Transport Layer Security)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configuration de l’ordre de la suite de chiffrement TLS

Les différentes versions de Windows prennent en charge différentes suites de chiffrement TLS et ordre de priorité. Pour connaître l’ordre par défaut pris en charge par le fournisseur Microsoft Schannel dans différentes versions de Windows, consultez [suites de chiffrement dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) .

> [!NOTE] 
> Vous pouvez également modifier la liste des suites de chiffrement à l’aide des fonctions CNG. pour plus d’informations, consultez [hiérarchisation des suites de chiffrement Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) .

Les modifications apportées à l’ordre de la suite de chiffrement TLS prennent effet au prochain démarrage. Jusqu’au redémarrage ou à l’arrêt, la commande existante sera appliquée.

> [!WARNING] 
> La mise à jour des paramètres du Registre pour l’ordre de priorité par défaut n’est pas prise en charge et peut être réinitialisée avec les mises à jour de maintenance. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configuration de l’ordre de la suite de chiffrement TLS à l’aide de stratégie de groupe

Vous pouvez utiliser les paramètres de stratégie de groupe ordre de la suite de chiffrement SSL pour configurer l’ordre de la suite de chiffrement TLS par défaut.

1. Dans la console de gestion des stratégies de groupe, accédez à **configuration** > ordinateur**modèles d’administration** > **réseaux** > **paramètres de configuration SSL**.
2. Double-cliquez sur ordre de la **suite de chiffrement SSL**, puis cliquez sur l’option **activé** .
3. Cliquez avec le bouton droit sur la zone **suites de chiffrement SSL** , puis sélectionnez **Sélectionner tout** dans le menu contextuel.

   ![Paramètre de stratégie de groupe](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Cliquez avec le bouton droit sur le texte sélectionné, puis sélectionnez **copier** dans le menu contextuel.
5. Collez le texte dans un éditeur de texte tel que Notepad. exe et mettez-le à jour avec la nouvelle liste de commandes de la suite de chiffrement.

   > [!NOTE]
   > La liste de commandes de la suite de chiffrement TLS doit être au format délimité par des virgules strict. Chaque chaîne de suite de chiffrement se termine par une virgule (,) à droite de celle-ci. 
   > 
   > En outre, la liste des suites de chiffrement est limitée à 1 023 caractères.

6. Remplacez la liste dans les **suites de chiffrement SSL** par la liste ordonnée mise à jour.
7. Cliquez sur **OK** ou **Appliquer**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configuration de l’ordre de la suite de chiffrement TLS à l’aide de MDM

Le CSP de la stratégie Windows 10 prend en charge la configuration des suites de chiffrement TLS. Pour plus d’informations, consultez la page [chiffrement/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) .

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configuration de l’ordre de la suite de chiffrement TLS à l’aide des applets de commande PowerShell TLS

Le module PowerShell TLS prend en charge l’obtention de la liste triée des suites de chiffrement TLS, la désactivation d’une suite de chiffrement et l’activation d’une suite de chiffrement. Pour plus d’informations, consultez le [module TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) .

## <a name="configuring-tls-ecc-curve-order"></a>Configuration de l’ordre des courbes ECC TLS 

À partir de Windows 10 & Windows Server 2016, l’ordre des courbes ECC peut être configuré indépendamment de l’ordre de la suite de chiffrement. Si la liste de commandes de la suite de chiffrement TLS possède des suffixes de courbe elliptique, ceux-ci sont remplacés par le nouvel ordre de priorité de la courbe elliptique, lorsqu’ils sont activés. Cela permet aux organisations d’utiliser un objet stratégie de groupe pour configurer différentes versions de Windows avec la même commande de suites de chiffrement.

> [!NOTE]
> Avant Windows 10, les chaînes de suite de chiffrement étaient ajoutées à la courbe elliptique pour déterminer la priorité de la courbe.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Gestion des courbes ECC Windows à l’aide de CertUtil

À compter de Windows 10 et Windows Server 2016, Windows fournit une gestion des paramètres de courbe elliptique par le biais de l’utilitaire de ligne de commande certutil. exe. Les paramètres de courbe elliptique sont stockés dans bcryptprimitives. dll. À l’aide de Certutil. exe, les administrateurs peuvent ajouter et supprimer des paramètres de courbe vers et à partir de Windows, respectivement. Certutil. exe stocke les paramètres de la courbe en toute sécurité dans le registre. Windows peut commencer à utiliser les paramètres de courbe par le nom associé à la courbe.    

#### <a name="displaying-registered-curves"></a>Affichage des courbes inscrites

Utilisez la commande certutil. exe suivante pour afficher la liste des courbes inscrites pour l’ordinateur actuel.

```powershell
certutil.exe –displayEccCurve
```

![Certutil afficher les courbes](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figure 1 sortie certutil. exe pour afficher la liste des courbes inscrites.*

#### <a name="adding-a-new-curve"></a>Ajout d’une nouvelle courbe

Les organisations peuvent créer et utiliser des paramètres de courbe étudiés par d’autres entités approuvées.  
Les administrateurs souhaitant utiliser ces nouvelles courbes dans Windows doivent ajouter la courbe.  
Utilisez la commande certutil. exe suivante pour ajouter une courbe à l’ordinateur actuel :

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- L’argument **curveName** représente le nom de la courbe sous laquelle les paramètres de la courbe ont été ajoutés.
- L’argument **curveParameters** représente le nom de fichier d’un certificat qui contient les paramètres des courbes que vous souhaitez ajouter.
- L’argument **curveOid** représente le nom de fichier d’un certificat contenant l’OID des paramètres de courbe que vous souhaitez ajouter (facultatif).
- L’argument **curveType** représente une valeur décimale de la courbe nommée du [Registre de la courbe nommée EC](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facultatif).

![Certutil-ajouter des courbes](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figure 2 Ajout d’une courbe à l’aide de Certutil. exe.*

#### <a name="removing-a-previously-added-curve"></a>Suppression d’une courbe précédemment ajoutée

Les administrateurs peuvent supprimer une courbe précédemment ajoutée à l’aide de la commande certutil. exe suivante :

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows ne peut pas utiliser une courbe nommée après qu’un administrateur a supprimé la courbe de l’ordinateur.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Gestion des courbes ECC Windows à l’aide de stratégie de groupe

Les organisations peuvent distribuer des paramètres de courbe à l’entreprise, à un ordinateur joint à un domaine, à l’aide de stratégie de groupe et à l’extension de registre des préférences de stratégie de groupe.  
Le processus de distribution d’une courbe est le suivant :

1.  Sur Windows 10 et Windows Server 2016, utilisez **certutil. exe** pour ajouter une nouvelle courbe nommée inscrite à Windows.
2.  À partir de ce même ordinateur, ouvrez la Console de gestion des stratégies de groupe (GPMC), créez un nouvel objet stratégie de groupe, puis modifiez-le.
3.  Accédez à **Configuration ordinateur | Préférences | Paramètres Windows | Registre**.  Cliquez avec le bouton droit sur **Registre**. Pointez sur **nouveau** , puis sélectionnez **élément de collecte**. Renommez l’élément de collecte pour qu’il corresponde au nom de la courbe. Vous allez créer un élément de collecte de Registre pour chaque clé de Registre sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurez la collection de registres de préférences de stratégie de groupe nouvellement créée en ajoutant un nouvel **élément de Registre** pour chaque valeur de Registre listée sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\[curveName ]* .
5.  Déployez l’objet stratégie de groupe contenant stratégie de groupe élément de collection de Registre sur les ordinateurs Windows 10 et Windows Server 2016 qui doivent recevoir les nouvelles courbes nommées.

    ![PRÉFÉRENCES distribuer des courbes](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figure 3 utilisation des préférences de stratégie de groupe pour distribuer des courbes*

## <a name="managing-tls-ecc-order"></a>Gestion de l’ordre ECC TLS

À compter de Windows 10 et de Windows Server 2016, les paramètres de stratégie de groupe de l’ordre ECC peuvent être utilisés pour configurer l’ordre de courbe ECC TLS par défaut. À l’aide de l’ECC générique et de ce paramètre, les organisations peuvent ajouter leurs propres courbes nommées approuvées (qui sont approuvées pour une utilisation avec TLS) au système d’exploitation, puis ajouter ces courbes nommées au paramètre de priorité de la courbe stratégie de groupe pour s’assurer qu’elles sont utilisées dans le TLS futur établissements. Les listes de nouvelles priorités de la courbe deviennent actives au redémarrage suivant après réception des paramètres de stratégie.     

![PRÉFÉRENCES distribuer des courbes](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figure 4 gestion de la priorité de la courbe TLS à l’aide de stratégie de groupe*


