---
title: "Gérer le Transport Layer Security (TLS)"
description: "Sécurité de Windows Server"
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
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>Gérer le Transport Layer Security (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>Ordre des suites de chiffrement TLS Configuration

Différentes versions de Windows prennent en charge différentes suites de chiffrement TLS et l’ordre de priorité. Voir [Suites de chiffrement de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) pour l’ordre par défaut pris en charge par le Microsoft Schannel Provider dans les différentes versions de Windows.

> [!NOTE] 
> Vous pouvez également modifier la liste des suites de chiffrement à l’aide des fonctions CNG, voir [hiérarchisation des Suites de chiffrement Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) pour plus d’informations.

Modifications apportées à l’ordre des suites de chiffrement TLS prendront effet lors du prochain démarrage. Jusqu'à ce que le redémarrage ou l’arrêt, la commande existante sera en vigueur.

> [!WARNING] 
> Mise à jour les paramètres du Registre pour l’ordre de priorité par défaut n’est pas prise en charge et peut être réinitialisé avec les mises à jour de maintenance. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurer l’ordre des suites de chiffrement TLS à l’aide de stratégie de groupe

Vous pouvez utiliser les paramètres de stratégie de groupe SSL chiffrement Suite ordre pour configurer l’ordre des suites de chiffrement TLS par défaut.

1.  À partir de la Console de gestion de stratégie de groupe, accédez à **Configuration ordinateur** > **modèles d’administration** > **réseaux** > **les paramètres de Configuration de SSL**.
2.  Double-cliquez sur **ordre des suites de chiffrement SSL**, puis cliquez sur le **activé** option.
3.  Avec le bouton droit **Suites de chiffrement SSL** et sélectionnez **sélectionner tout** dans le menu contextuel.

    ![Paramètre de stratégie de groupe](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  Cliquez sur le texte sélectionné, puis sélectionnez **copie** dans le menu contextuel.
5.  Collez le texte dans un éditeur de texte tels que notepad.exe et mise à jour avec la nouvelle liste de commande suite de chiffrement.

    > [!NOTE]
    > La liste de commande de suite de chiffrement TLS doit être au format strict par des virgules. Chaque chaîne suite de chiffrement se termine par une virgule (,) sur le côté droit de celui-ci. 

    > En outre, la liste des suites de chiffrement est limitée à 1023 caractères.

6.  Remplacement de la liste dans le **Suites de chiffrement SSL** avec la liste ordonnée mis à jour.
7.  Cliquez sur **OK** ou **appliquer**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configurer l’ordre des suites de chiffrement TLS à l’aide de GPM

Le fournisseur CSP Policy de Windows 10 prend en charge la configuration des Suites de chiffrement TLS. Voir [cryptographie/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) pour plus d’informations.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurer l’ordre des suites de chiffrement TLS à l’aide des applets de commande PowerShell TLS

Le module PowerShell TLS prend en charge la mise en route de la liste ordonnée des suites de chiffrement TLS, en désactivant une suite de chiffrement et l’activation d’une suite de chiffrement. Voir [Module TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) pour plus d’informations.

## <a name="configuring-tls-ecc-curve-order"></a>Configuration de l’ordre de courbe ECC TLS 

À partir de Windows 10 et Windows Server 2016, commande de courbe ECC peut être configuré indépendamment de l’ordre des suites de chiffrement. Si le protocole TLS liste a des suffixes de chiffrement à courbe elliptique ordre des suites de chiffrement, ils sont remplacés par le nouvel ordre de priorité à courbe elliptique, lorsque activé. Cela permet aux organisations d’utiliser un objet de stratégie de groupe pour configurer les différentes versions de Windows avec le même ordre de suites de chiffrement.

> [!NOTE]
> Avant Windows 10, les chaînes de suite de chiffrement ont été ajoutées avec le chiffrement à courbe elliptique pour déterminer la priorité de la courbe.

### <a name="managing-windows-ecc-curves-using-certutil"></a>La gestion des courbes ECC Windows à l’aide de CertUtil

À partir de Windows 10 et Windows Server 2016, Windows fournit gestion des paramètres de chiffrement à courbe elliptique grâce à la certuil.exe Utilitaire ligne de commande. Paramètres de chiffrement à courbe elliptique sont stockés dans le bcryptprimitives.dll. À l’aide de certutil.exe, les administrateurs peuvent ajouter et supprimer des paramètres de courbe vers et à partir de Windows, respectivement. Certutil.exe stocke les paramètres de courbe en toute sécurité dans le Registre. Windows peut commencer à l’aide de paramètres de la courbe par le nom de la courbe.    

#### <a name="displaying-registered-curves"></a>Affichage des courbes

Utilisez la commande certutil.exe pour afficher une liste des courbes enregistré pour l’ordinateur actuel.

```powershell
certutil.exe –displayEccCurve
```

![Certutil affichage courbes](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figure 1 Certutil.exe pour afficher la liste des courbes inscrits de sortie.*

#### <a name="adding-a-new-curve"></a>Ajout d’une nouvelle courbe

Les organisations peuvent créer et utiliser les paramètres de courbe effectuer des recherches par d’autres entités approuvées.  
Les administrateurs qui souhaitent utiliser ces nouvelles courbes dans Windows doivent ajouter la courbe.  
Pour ajouter une courbe à ordinateur actuel, utilisez la commande certutil.exe suivante:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- Le **la courbe name** argument représente le nom de la courbe sous lequel les paramètres de courbe ont été ajoutés.
- Le **curveParameters** argument représente le nom de fichier d’un certificat qui contient les paramètres des courbes que vous souhaitez ajouter.
- Le **curveOid** argument représente un nom de fichier d’un certificat qui contient l’OID des paramètres de courbe que vous souhaitez ajouter (facultatif).
- Le **curveType** argument représente une valeur décimale de la courbe nommée à partir de la [ce nommé courbe Registre](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facultatif).

![Ajouter des courbes de Certutil](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figure 2 Ajout d’une courbe à l’aide de certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Suppression d’une courbe ajoutée précédemment

Les administrateurs peuvent supprimer une courbe ajoutée précédemment à l’aide de la commande certutil.exe suivante:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows ne peut pas utiliser une courbe nommée après qu’un administrateur supprime la courbe d’ordinateur.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>La gestion des courbes ECC Windows à l’aide de la stratégie de groupe

Organisations peuvent distribuer des paramètres de courbe dans l’entreprise, joints au domaine, ordinateur à l’aide de la stratégie de groupe et l’extension du Registre de préférences de stratégie de groupe.  
Le processus de distribution d’une courbe est:

1.  Sur Windows 10 et Windows Server 2016, utilisez **certutil.exe** pour ajouter une nouvelle courbe nommée inscrite à Windows.
2.  À partir de ce même ordinateur, ouvrez la Console de gestion des stratégies de groupe (GPMC), créer un nouvel objet de stratégie de groupe et la modifier.
3.  Accédez à **Configuration ordinateur | Préférences | Paramètres Windows | Registre**.  Avec le bouton droit **Registre**. Survolez **New** et sélectionnez **élément de Collection**. Renommer l’élément de collection pour correspondre au nom de la courbe. Vous allez créer un élément de Collection de Registre pour chaque clé de Registre sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurer la Collection de Registre de préférence de stratégie groupe nouvellement créé en ajoutant un nouveau **élément de Registre** pour chaque valeur de Registre répertoriées sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [la courbe name]*.
5.  Déployer l’objet de stratégie de groupe contenant l’élément de Collection de Registre de stratégie de groupe aux ordinateurs Windows 10 et Windows Server 2016 qui doivent recevoir les nouvelles courbes nommées.

    ![GPP distribuer courbes](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figure 3 à l’aide de stratégie de groupe préférences pour distribuer des courbes*

## <a name="managing-tls-ecc-order"></a>Gestion de la commande TLS ECC

À partir de Windows 10 et Windows Server 2016, la stratégie de groupe de commande de courbe ECC paramètres peuvent être utilisés configurer la commande de courbe ECC TLS par défaut. À l’aide d’ECC générique et ce paramètre, les organisations permettre ajouter leurs propres nommé courbes (qui sont approuvées pour une utilisation avec le protocole TLS) pour le système d’exploitation approuvé et puis ajoutez-les nommé courbes pour le paramètre de stratégie de groupe de priorité de courbe pour vous assurer qu’ils sont utilisés dans les futures négociations TLS. Nouvelles listes de priorité de courbe deviennent actives sur le prochain redémarrage après avoir reçu les paramètres de stratégie.     

![GPP distribuer courbes](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figure 4 gestion TLS courbe priorité à l’aide de la stratégie de groupe*


