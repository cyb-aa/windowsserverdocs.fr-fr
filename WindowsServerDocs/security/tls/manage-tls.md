---
title: Gérer la sécurité de couche Transport (TLS)
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
ms.openlocfilehash: 872647f09898bf8ae08ee69f28b717d28abf7c78
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447305"
---
# <a name="manage-transport-layer-security-tls"></a>Gérer la sécurité de couche Transport (TLS)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configuration ordre des suites de chiffrement TLS

Différentes versions de Windows prennent en charge différentes suites de chiffrement TLS et ordre de priorité. Consultez [Suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) pour l’ordre par défaut pris en charge par le Microsoft Schannel Provider dans différentes versions de Windows.

> [!NOTE] 
> Vous pouvez également modifier la liste des suites de chiffrement à l’aide des fonctions du CNG, consultez [hiérarchisation des Suites de chiffrement Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) pour plus d’informations.

Modifications apportées à l’ordre des suites de chiffrement TLS prendront effet au prochain redémarrage. Jusqu'à ce que le redémarrage ou l’arrêt, la commande existante sera appliquée.

> [!WARNING] 
> La mise à jour les paramètres du Registre pour l’ordre de priorité par défaut n’est pas pris en charge et peut être réinitialisé avec mises à jour de maintenance. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configuration d’ordre des suites de chiffrement TLS à l’aide de stratégie de groupe

Vous pouvez utiliser les paramètres de stratégie de groupe de l’ordre de SSL Cipher Suite pour configurer l’ordre des suites TLS par défaut.

1. À partir de la Console de gestion de stratégie de groupe, accédez à **Configuration ordinateur** > **modèles d’administration** > **réseaux**  >  **Les paramètres de Configuration de SSL**.
2. Double-cliquez sur **ordre des suites de chiffrement SSL**, puis cliquez sur le **activé** option.
3. Avec le bouton droit **Suites de chiffrement SSL** et sélectionnez **sélectionner tout** dans le menu contextuel.

   ![Paramètre de stratégie de groupe](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Cliquez sur le texte sélectionné, puis sélectionnez **copie** dans le menu contextuel.
5. Collez le texte dans un éditeur de texte comme notepad.exe et mise à jour avec la nouvelle liste de commande de suite de chiffrement.

   > [!NOTE]
   > La liste des commandes TLS cipher suite doit être au format de strict par des virgules. Chaque chaîne de suite de chiffrement s’achèvera par une virgule (,) à droite de celui-ci. 
   > 
   > En outre, la liste des suites de chiffrement est limitée à 1 023 caractères.

6. Remplacement de la liste dans le **Suites de chiffrement SSL** avec la liste ordonnée mis à jour.
7. Cliquez sur **OK** ou **Appliquer**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configuration d’ordre des suites de chiffrement TLS à l’aide de gestion des appareils mobiles

Le CSP de stratégie Windows 10 prend en charge la configuration des Suites de chiffrement TLS. Consultez [cryptographie/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) pour plus d’informations.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configuration d’ordre des suites de chiffrement TLS à l’aide des applets de commande PowerShell TLS

Le module PowerShell de TLS prend en charge l’obtention de la liste ordonnée des suites de chiffrement TLS, la désactivation d’une suite de chiffrement et l’activation d’une suite de chiffrement. Consultez [TLS Module](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) pour plus d’informations.

## <a name="configuring-tls-ecc-curve-order"></a>Configuration de l’ordre de courbe ECC de TLS 

À partir de Windows 10 et Windows Server 2016, ordre de courbe ECC peut être configuré indépendamment de l’ordre des suites de chiffrement. Si la liste comporte les suffixes de courbe elliptique l’ordre des suites de chiffrement le TLS, ils seront remplacées par la nouvelle commande de priorité de courbe elliptique lorsque activé. Cela permettent aux organisations d’utiliser un objet de stratégie de groupe pour configurer les différentes versions de Windows avec le même ordre de suites de chiffrement.

> [!NOTE]
> Avant Windows 10, cipher suite chaînes ont été ajoutées à la courbe elliptique à déterminer la priorité de la courbe.

### <a name="managing-windows-ecc-curves-using-certutil"></a>La gestion des courbes ECC de Windows à l’aide de CertUtil

Depuis Windows 10 et Windows Server 2016, Windows fournit la gestion des paramètres de courbe elliptique via le certutil.exe Utilitaire ligne de commande. Paramètres de courbe elliptique sont stockés dans le bcryptprimitives.dll. À l’aide de certutil.exe, les administrateurs peuvent ajouter et supprimer des paramètres de la courbe vers et à partir de Windows, respectivement. Certutil.exe stocke les paramètres de courbe en toute sécurité dans le Registre. Windows peut commencer à l’aide des paramètres de la courbe par le nom associé à la courbe.    

#### <a name="displaying-registered-curves"></a>Affichage des courbes

Utilisez la commande certutil.exe suivante pour afficher une liste des courbes inscrit pour l’ordinateur actuel.

```powershell
certutil.exe –displayEccCurve
```

![Certutil affichage courbes](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figure 1 les Certutil.exe pour afficher la liste des courbes inscrits de sortie.*

#### <a name="adding-a-new-curve"></a>Ajout d’une nouvelle courbe

Les organisations peuvent créer et utiliser les paramètres de courbe examinées par d’autres entités approuvées.  
Les administrateurs qui souhaitent utiliser ces nouvelles courbes dans Windows doivent ajouter la courbe.  
Pour ajouter une courbe de l’ordinateur actuel, utilisez la commande certutil.exe suivante :

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- Le **la courbe name** argument représente le nom de la courbe sous lequel les paramètres de courbe ont été ajoutés.
- Le **curveParameters** argument représente le nom de fichier d’un certificat qui contient les paramètres des courbes que vous souhaitez ajouter.
- Le **curveOid** argument représente un nom de fichier d’un certificat qui contient l’OID des paramètres de courbe que vous souhaitez ajouter (facultatif).
- Le **curveType** argument représente une valeur décimale de la courbe nommée à partir de la [EC nommé courbe Registre](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (facultatif).

![Ajouter des courbes de Certutil](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figure 2 Ajout d’une courbe à l’aide de certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Suppression d’une courbe précédemment ajoutée

Les administrateurs peuvent supprimer une courbe précédemment ajoutée à l’aide de la commande certutil.exe suivante :

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows ne peut pas utiliser une courbe nommée une fois un administrateur supprime la courbe à partir de l’ordinateur.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>La gestion des courbes ECC de Windows à l’aide de la stratégie de groupe

Les organisations peuvent distribuer les paramètres de la courbe dans l’entreprise, à un domaine, ordinateur à l’aide de la stratégie de groupe et l’extension de Registre de préférences de stratégie de groupe.  
Le processus de distribution d’une courbe est :

1.  Sur Windows 10 et Windows Server 2016, utilisez **certutil.exe** pour ajouter une courbe nommée inscrite nouvelle à Windows.
2.  À partir de ce même ordinateur, ouvrez la Console de gestion des stratégies de groupe (GPMC), créez un nouvel objet de stratégie de groupe et le modifier.
3.  Accédez à **Configuration ordinateur | Préférences | Les paramètres Windows | Registre**.  Avec le bouton droit **Registre**. Placez le curseur sur **New** et sélectionnez **élément de Collection**. Renommer l’élément de collecte pour correspondre au nom de la courbe. Vous allez créer un élément de collecte du Registre pour chaque clé de Registre sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurer la Collection de Registre de préférence de stratégie groupe nouvellement créé en ajoutant une nouvelle **élément de Registre** pour chaque valeur de Registre répertoriée sous *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ ECCParameters\[la courbe name]* .
5.  Déployer l’objet de stratégie de groupe contenant l’élément de Collection de Registre de stratégie de groupe aux ordinateurs Windows 10 et Windows Server 2016 qui doit recevoir les nouvelles courbes nommées.

    ![GPP distribuer des courbes](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figure 3 à l’aide de stratégie de groupe préférences pour distribuer des courbes*

## <a name="managing-tls-ecc-order"></a>Gestion de la commande TLS ECC

Depuis Windows 10 et Windows Server 2016, la stratégie de groupe d’ordre de courbe ECC paramètres peuvent être utilisés configurer l’ordre de courbe ECC TLS par défaut. À l’aide d’ECC générique et que ce paramètre, les organisations permettre ajouter leurs propres approuvé nommé courbes (qui sont approuvés pour une utilisation avec TLS) pour le système d’exploitation, puis ajouter ces courbes nommés pour le paramètre de stratégie de groupe de priorité de courbe pour vous assurer qu’ils sont utilisés dans les futures TLS établissements de liaisons. Nouvelles listes de priorité de courbe deviennent actives sur le prochain redémarrage après avoir reçu les paramètres de stratégie.     

![GPP distribuer des courbes](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figure 4 la gestion TLS courbe priorité à l’aide de la stratégie de groupe*


