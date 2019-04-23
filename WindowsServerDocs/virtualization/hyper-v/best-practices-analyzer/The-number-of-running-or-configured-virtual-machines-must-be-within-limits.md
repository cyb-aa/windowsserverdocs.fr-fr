---
title: Le nombre d’en cours d’exécution ou machines virtuelles configurées doivent se trouver dans les limites prises en charge
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a971a48b2d8199a6c279f1bd3f1715039fa6e0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855350"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>Le nombre d’en cours d’exécution ou machines virtuelles configurées doivent se trouver dans les limites prises en charge

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Plus de machines virtuelles sont en cours d’exécution ou configurés que sont pris en charge.*  
  
## <a name="impact"></a>Impact  
*Microsoft ne prend pas en charge le nombre actuel de machines virtuelles en cours d’exécution ou configuré sur ce serveur.*  
  
## <a name="resolution"></a>Résolution  
*Déplacer un ou plusieurs ordinateurs virtuels vers un autre serveur.*  
  
Pour plus d’informations sur les configurations prises en charge maximales pour Hyper-V, comme le nombre de machines virtuelles en cours d’exécution, consultez [planifier extensibilité Hyper-V dans Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Pour déplacer un ordinateur virtuel vers un autre serveur, vous pouvez :  
  
- Exporter l’ordinateur virtuel à partir du serveur en cours et les importer vers un nouveau serveur comme décrit ci-dessous.   
- Effectuez une migration dynamique :   
    - Si ce serveur appartienne à un cluster de basculement, utilisez les outils fournis avec la fonctionnalité de Clustering de basculement. Pour obtenir des instructions, consultez [Live migrer, migration rapide ou déplacer un ordinateur virtuel à partir du nœud à nœud](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - S’il s’agit d’un serveur autonome, consultez les instructions dans [configurer la Migration en direct et migration d’ordinateurs virtuels sans Clustering de basculement](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Pour exporter une machine virtuelle  
  
   > [!IMPORTANT]  
   > Si l’hôte Hyper-V que vous exportez à partir d’appartient à un domaine et que vous souhaitez stocker les fichiers exportés dans un emplacement distant, l’hôte Hyper-V doit être configuré pour la délégation contrainte. Un emplacement distant peut être un dossier réseau partagé ou un dossier sur l’ordinateur hôte que vous importez à. La délégation contrainte permet le compte d’ordinateur de l’hôte Hyper-V fournissent des informations d’identification déléguées pour le service de fichier système CIFS (Common Internet) à l’ordinateur distant. Pour obtenir des instructions sur la configuration de la délégation contrainte, consultez la section suivante de l’exportation et importer des instructions, ci-dessous.  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **Machines virtuelles**, cliquez sur une machine virtuelle, puis sur **exporter**.  
  
3.  Dans le **exporter une Machine virtuelle** boîte de dialogue, tapez ou accédez à un emplacement qui dispose de suffisamment d’espace libre pour stocker toutes les ressources de machine virtuelle. Lorsque vous exportez une machine virtuelle, tous les disques durs virtuels (fichiers .vhd ou .vhdx fichiers), points de contrôle (fichiers .avhd) et les fichiers d’état enregistré associés à la machine virtuelle sont copiés dans le dossier spécifié.  
  
4.  Cliquez sur **Exporter**.  
  
Après avoir exporté les ordinateurs virtuels, importer les ordinateurs virtuels à l’autre serveur.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Pour importer un ordinateur virtuel vers un autre serveur  
  
1.  Connectez-vous au serveur exécutant Hyper-V et ouvrez le Gestionnaire Hyper-V.  
  
2.  Dans le **Action** volet, cliquez sur **importer l’ordinateur virtuel**.  
  
3.  Dans le **importer l’ordinateur virtuel** boîte de dialogue, spécifiez l’emplacement où vous avez exporté la machine virtuelle. Sauf si vous souhaitez importer à nouveau cette machine virtuelle, laissez les paramètres d’importation en l’état.  
  
4.  Cliquez sur **Importer**.  
  
### <a name="to-configure-constrained-delegation"></a>Pour configurer la délégation contrainte  
  
L’appartenance à la **administrateurs de domaine** groupe est requis pour effectuer cette procédure.  
  
1.  Sur un ordinateur muni des outils des Services de domaine Active Directory, en **outils d’administration**, ouvrez **Active Directory Users and Computers**, puis accédez au compte d’ordinateur pour l’ordinateur exécutant Hyper-V.  
  
    > [!NOTE]  
    > Si **Utilisateurs et ordinateurs Active Directory** n'est pas répertorié, installez la fonction Outils des Services de domaine Active Directory. Pour obtenir des instructions, consultez [l’installation de Server Outils d’Administration distant pour AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  Cliquez sur le compte d’ordinateur pour l’ordinateur exécutant Hyper-V, puis cliquez sur **propriétés**.  
  
3.  Sous l'onglet **Délégation**, cliquez sur **N'approuver cet ordinateur que pour la délégation aux services spécifiés**, puis cliquez sur **Utiliser tout protocole d'authentification**.  
  
4.  Pour autoriser le compte d’ordinateur Hyper-V présenter des informations d’identification déléguées à l’ordinateur distant :  
  
    1.  Cliquez sur **Ajouter**.  
  
    2.  Dans le **ajouter des Services** boîte de dialogue, cliquez sur **utilisateurs ou ordinateurs**, sélectionnez l’ordinateur distant, puis cliquez sur **OK**.  
  
    3.  Dans le **services disponibles** liste, sélectionnez le **cifs** de protocole (également appelé le protocole Server Message Block (SMB)), puis cliquez sur **ajouter**.  
  
  
  


