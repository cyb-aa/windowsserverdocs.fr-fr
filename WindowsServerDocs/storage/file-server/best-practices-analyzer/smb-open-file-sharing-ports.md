---
title: Les ports de partage de fichiers et d’imprimantes SMB doivent être ouverts
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: a7e98129c2fb4f2259364c547b426d46f0a24ef3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859462"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB : le fichier et les ports de partage de l’imprimante doivent être ouverts


Mise à jour : 2 février 2011

S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, Windows Server 2008 R2

*Cette rubrique vise à résoudre un problème spécifique identifié par une analyse Best Practices Analyzer. Vous devez appliquer les informations contenues dans cette rubrique uniquement aux ordinateurs sur lesquels les services de fichiers Best Practices Analyzer exécutés et qui rencontrent le problème traité dans cette rubrique. Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Système d'exploitation</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>Produit/fonctionnalité</strong></p></td>
<td><p>Services de fichiers</p></td>
</tr>
<tr class="odd">
<td><p><strong>Va</strong></p></td>
<td><p>Error</p></td>
</tr>
<tr class="even">
<td><p><strong>Catégorie</strong></p></td>
<td><p>Configuration</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problème

> *Les ports de pare-feu nécessaires pour le partage de fichiers et d’imprimantes ne sont pas ouverts (ports 445 et 139).*

## <a name="impact"></a>Impact

> *Les ordinateurs ne seront pas en mesure d’accéder aux dossiers partagés et à d’autres services réseau SMB (Server Message Block) sur ce serveur.*

## <a name="resolution"></a>Résolution

> *Activez le partage de fichiers et d’imprimantes pour communiquer via le pare-feu de l’ordinateur.*

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d’un groupe équivalent.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Pour ouvrir les ports du pare-feu pour activer le partage de fichiers et d’imprimantes

1.  Ouvrez le panneau de configuration, cliquez sur **système et sécurité**, puis sur **pare-feu Windows**.

2.  Dans le volet gauche, cliquez sur **Paramètres avancés**, puis dans l’arborescence de la console, cliquez sur **règles de trafic entrant**.

3.  Sous **règles de trafic entrant**, localisez le fichier de règles **et le partage d’imprimante (NB-session-entrée)** et le **partage de fichiers et d’imprimantes (SMB-in)** .

4.  Pour chaque règle, cliquez avec le bouton droit sur la règle, puis cliquez sur **activer la règle**.

## <a name="additional-references"></a>Références supplémentaires

[Fonctionnement des dossiers partagés et du pare-feu Windows](https://technet.microsoft.com/library/cc731402.aspx)(https://technet.microsoft.com/library/cc731402.aspx)

