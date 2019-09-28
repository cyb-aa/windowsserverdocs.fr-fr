---
title: Le nombre d’ordinateurs virtuels en cours d’exécution ou configurés doit être compris dans les limites de prise en charge
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 56d7fd528d7fda20dbdbb16a6262bb072f053ef0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364628"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>Le nombre d’ordinateurs virtuels en cours d’exécution ou configurés doit être compris dans les limites de prise en charge

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Davantage d’ordinateurs virtuels sont en cours d’exécution ou configurés que ce qui est pris en charge.*  
  
## <a name="impact"></a>Impact  
*Microsoft ne prend pas en charge le nombre actuel de machines virtuelles en cours d’exécution ou configurées sur ce serveur.*  
  
## <a name="resolution"></a>Résolution :  
*Déplacer un ou plusieurs ordinateurs virtuels vers un autre serveur.*  
  
Pour plus d’informations sur les configurations maximales prises en charge pour Hyper-V, telles que le nombre d’ordinateurs virtuels en cours d’exécution, consultez [planifier l’extensibilité d’Hyper-v dans Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Pour déplacer un ordinateur virtuel vers un autre serveur, vous pouvez :  
  
- Exportez l’ordinateur virtuel à partir du serveur actuel, puis importez-le sur un nouveau serveur, comme décrit ci-dessous.   
- Effectuer une migration dynamique :   
    - Si ce serveur appartient à un cluster de basculement, utilisez les outils fournis avec la fonctionnalité de clustering de basculement. Pour obtenir des instructions, consultez [migration dynamique, migration rapide ou déplacement d’un ordinateur virtuel d’un nœud à un nœud](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - S’il s’agit d’un serveur autonome, consultez [les instructions dans configurer migration dynamique et migration d’ordinateurs virtuels sans clustering de basculement](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Pour exporter un ordinateur virtuel  
  
   > [!IMPORTANT]  
   > Si l’hôte Hyper-V que vous exportez appartient à un domaine et que vous souhaitez stocker les fichiers exportés sur un emplacement distant, l’ordinateur hôte Hyper-V doit être configuré pour la délégation avec restriction. Un emplacement distant peut être un dossier réseau partagé ou un dossier sur l’ordinateur hôte sur lequel vous effectuez l’importation. La délégation avec restriction permet au compte d’ordinateur de l’hôte Hyper-V de fournir des informations d’identification déléguées pour le service CIFS (Common Internet File System) à l’ordinateur distant. Pour obtenir des instructions sur la configuration de la délégation avec restriction, consultez la section suivant les instructions d’exportation et d’importation, ci-dessous.  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **ordinateurs virtuels**, cliquez avec le bouton droit sur un ordinateur virtuel, puis cliquez sur **Exporter**.  
  
3.  Dans la boîte de dialogue **exporter un ordinateur virtuel** , tapez ou accédez à un emplacement disposant de suffisamment d’espace libre pour stocker toutes les ressources de l’ordinateur virtuel. Lorsque vous exportez un ordinateur virtuel, tous les disques durs virtuels (fichiers. vhd ou fichiers. vhdx), les points de contrôle (fichiers. avhd) et les fichiers d’État enregistrés associés à l’ordinateur virtuel sont copiés dans le dossier spécifié.  
  
4.  Cliquez sur **Exporter**.  
  
Après avoir exporté les machines virtuelles, importez les machines virtuelles sur l’autre serveur.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Pour importer un ordinateur virtuel sur un autre serveur  
  
1.  Connectez-vous au serveur exécutant Hyper-V et ouvrez le Gestionnaire Hyper-V.  
  
2.  Dans le volet **action** , cliquez sur **Importer un ordinateur virtuel**.  
  
3.  Dans la boîte de dialogue **importer l’ordinateur virtuel** , spécifiez l’emplacement où vous avez exporté l’ordinateur virtuel. À moins que vous ne souhaitiez réimporter cet ordinateur virtuel, laissez les paramètres d’importation tels quels.  
  
4.  Cliquez sur **Importer**.  
  
### <a name="to-configure-constrained-delegation"></a>Pour configurer la délégation contrainte  
  
Pour effectuer cette procédure, vous devez appartenir au groupe **administrateurs du domaine** .  
  
1.  Sur un ordinateur sur lequel la fonctionnalité outils Active Directory Domain Services est installée, dans **Outils d’administration**, ouvrez **Active Directory utilisateurs et ordinateurs**, puis accédez au compte d’ordinateur de l’ordinateur exécutant Hyper-V.  
  
    > [!NOTE]  
    > Si **Utilisateurs et ordinateurs Active Directory** n'est pas répertorié, installez la fonction Outils des Services de domaine Active Directory. Pour obtenir des instructions, consultez [installation de outils d’administration de serveur distant pour AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  Cliquez avec le bouton droit sur le compte d’ordinateur de l’ordinateur exécutant Hyper-V, puis cliquez sur **Propriétés**.  
  
3.  Sous l'onglet **Délégation**, cliquez sur **N'approuver cet ordinateur que pour la délégation aux services spécifiés**, puis cliquez sur **Utiliser tout protocole d'authentification**.  
  
4.  Pour permettre au compte d’ordinateur Hyper-V de présenter des informations d’identification déléguées à l’ordinateur distant :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Dans la boîte de dialogue **Ajouter des services** , cliquez sur **utilisateurs ou ordinateurs**, sélectionnez l’ordinateur distant, puis cliquez sur **OK**.  
  
    3.  Dans la liste **services disponibles** , sélectionnez le protocole **CIFS** (également appelé protocole SMB (Server Message Block)), puis cliquez sur **Ajouter**.  
  
  
  


