---
Title: 'SMB : Fichiers et imprimantes partage de ports doivent être ouverts'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: b6ad1f1f8573fc380e999e5ec2091cea8ebb8aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820360"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB : Fichiers et imprimantes partage de ports doivent être ouverts


Mise à jour : 2 février 2011

S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, Windows Server 2008 R2

*Cette rubrique est conçue pour résoudre un problème spécifique identifié par une analyse Best Practices Analyzer. Vous devez appliquer les informations contenues dans cette rubrique uniquement aux ordinateurs qui ont été fichier Services Best Practices Analyzer à exécutée et que vous rencontrez le problème décrit dans cette rubrique. Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


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
<td><p><strong>Niveau de gravité</strong></p></td>
<td><p>Erreur</p></td>
</tr>
<tr class="even">
<td><p><strong>Catégorie</strong></p></td>
<td><p>Configuration</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problème

> *Les ports de pare-feu nécessaires pour le partage de fichiers et imprimantes soient n'ouvrent pas (ports 445 et 139).*

## <a name="impact"></a>Impact

> *Ordinateurs ne seront pas en mesure d’accéder aux dossiers partagés et autres services réseau SMB Server Message Block sur ce serveur.*

## <a name="resolution"></a>Résolution

> *Activer le partage de fichiers et imprimantes communiquer via le pare-feu de l’ordinateur.*

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d’un groupe équivalent.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Pour ouvrir les ports du pare-feu pour activer le partage de fichiers et imprimantes

1.  Ouvrez le panneau de configuration, cliquez sur **système et sécurité**, puis cliquez sur **Windows Firewall**.

2.  Dans le volet gauche, cliquez sur **paramètres avancés**, dans l’arborescence de la console, cliquez sur **règles de trafic entrant**.

3.  Sous **règles de trafic entrant**, recherchez les règles **partage de fichiers et imprimantes (NB-Session-entrée)** et **partage de fichiers et imprimantes (SMB-entrée)**.

4.  Pour chaque règle, avec le bouton droit de la règle, puis cliquez sur **activer la règle**.

## <a name="additional-references"></a>Références supplémentaires

[Présentation des dossiers partagés et le pare-feu Windows](http://technet.microsoft.com/en-us/library/cc731402.aspx)()http://technet.microsoft.com/en-us/library/cc731402.aspx)

