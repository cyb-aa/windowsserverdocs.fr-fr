---
title: Azure Hybrid Benefit pour WindowsServer
description: Utilisez vos licences WindowsServer locales pour faire des économies sur les ordinateurs virtuels Azure
ms.prod: windows-server
ms.date: 11/10/2017
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.openlocfilehash: 62821abc6c9eec660fa6af832bb1aba151708021
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783671"
---
# Azure Hybrid Benefit pour WindowsServer

>S’applique à WindowsServer

## Description, règles et scénarios d’utilisation de l’avantage

Azure Hybrid Benefit pour WindowsServer vous permet d’économiser jusqu’à 40% sur les ordinateurs virtuels WindowsServer dans Azure en utilisant vos licences WindowsServer locales avec Software Assurance.  Grâce à cet avantage, les clients ne doivent payer que les coûts d’infrastructure de chaque ordinateur virtuel, car la licence WindowsServer est couverte par l’avantage Software Assurance.  Cet avantage s’applique aux éditions Standard et Datacenter de WindowsServer pour les versions 2008R2, 2012, 2012R2 et 2016.  Cet avantage est disponible dans les Clouds de toutes les régions et dans tous les Clouds souverains.


![image1](media/ahb01.png)

Pour bénéficier de cet avantage, il vous suffit de disposer d’une licence Software Assurance active ou d’une souscription telle que EAS, SCE ou Open Value avec les licences WindowsServer.  

Chaque licence WindowsServer pour 2processeurs avec une licence Software Assurance (SA) ou une souscription, et chaque jeu de 16licences WindowsServer de base avec Software Assurance ou une souscription octroie au client le droit d’utiliser WindowsServer sur MicrosoftAzure sur jusqu’à 16cœurs virtuels alloués sur deux instances de base Azure (ordinateurs virtuels). Chaque jeu supplémentaire de 8licences de base avec SA ou une souscription octroie un droit d’utilisation sur jusqu’à 8cœurs virtuels et une instance de base (ordinateur virtuel).

| Licence avec SA/souscription            | Ordinateurs virtuels et cœurs accordés            | Utilisation autorisée                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| WS Datacenter (16cœurs ou une licence pour 2processeurs)  | Jusqu’à deux ordinateurs virtuels et jusqu’à 16cœurs | Exécuter des ordinateurs virtuels à la fois localement et dans Azure  |
| WS Standard (16cœurs ou une licence pour 2processeurs)    | Jusqu’à deux ordinateurs virtuels et jusqu’à 16cœurs | Exécuter des ordinateurs virtuels soit localement, soit dans Azure |

Les ordinateurs virtuels utilisant Azure Hybrid Benefit ne peuvent s’exécuter dans Azure que pendant la durée du SA ou de la souscription. À l’approche de l’échéance du SA ou de la souscription, le client peut, au choix, renouveler son SA ou sa souscription, désactiver la fonctionnalité Hybrid Benefit pour l’ordinateur virtuel concerné ou mettre hors service l’ordinateur virtuel qui utilise Hybrid Benefit. 

### Exemples d’économies 

![image2](media/ahb02.png)
 
Vous trouverez ci-dessous un tableau de référence pour vous aider à comprendre les règles des avantages de manière plus détaillée. La colonne verte indique la quantité d’ordinateurs virtuels du même type et la rangée bleue indique la densité de cœurs de chaque ordinateur virtuel. Les cellules jaunes indiquent le nombre de licences pour 2processeurs (ou jeux de 16cœurs) nécessaire pour déployer un certain nombre d’ordinateurs virtuels d’une certaine densité de cœurs. 

Tableau de référence des conditions requises pour WindowsServer avec SA:

![image3](media/ahb03.png)
 
Azure Hybrid Benefit pour WindowsServer vous permet également d’exécuter des configurations en fonction de vos besoins, ainsi que de combiner des ordinateurs virtuels de différents types.

Exemples de configurations pour plusieurs positionnements de licence:

![image 4](media/ahb04.png)
![image 5](media/ahb05.png)

 
Si vous souhaitez en savoir plus sur Azure Hybrid Benefit pour WindowsServer, accédez au site Web Azure Hybrid Benefit.

## Comment rester en conformité

Les clients qui souhaitent appliquer Azure Hybrid Benefit à leurs ordinateurs virtuels WindowsServer doivent vérifier le nombre de licences éligibles et la période de couverture respective de leur SA ou de leur souscription avant d’activer l’avantage. Ils doivent en outre suivre les recommandations ci-dessus pour déployer le nombre approprié d’ordinateurs virtuels bénéficiant de l’avantage. Si vous avez déjà des ordinateurs virtuels en cours d’exécution avec Azure Hybrid Benefit, vous devez inventorier le nombre d’unités en cours d’exécution et les comparer aux licences SA actives dont vous disposez.  Veuillez contacter votre spécialiste des licences Contrat Entreprise Microsoft pour valider votre positionnement de licence SA.
Pour afficher et dénombrer tous les ordinateurs virtuels déployés avec Azure Hybrid Benefit pour WindowsServer dans le cadre d’une souscription, vous pouvez effectuer l’une des actions ci-dessous:

1. configurer le portail MicrosoftAzure pour qu’il affiche l’utilisation de Azure Hybrid Benefit WindowsServer; ajoutez la colonne «Azure Hybrid Benefit» dans la vue de liste de la section des ordinateurs virtuels dans le portail MicrosoftAzure; 

    ![image6](media/ahb06.png)

2.  utiliser PowerShell pour répertorier l’utilisation de Azure Hybrid Benefit pour WindowsServer;

    ```
    $vms = Get-AzureRMVM 
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  examiner votre facture MicrosoftAzure pour déterminer le nombre d’ordinateurs virtuels en cours d’exécution disposant de Azure Hybrid Benefit; Les informations relatives au nombre d’instances disposant de l’avantage s’affichent sous «Informations supplémentaires»:

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}" 
    ```

Veuillez noter que la facturation ne s’applique pas en temps réel. Autrement dit, il y aura quelques heures de délai entre le moment où vous aurez activé un ordinateur virtuel avec Hybrid Benefit et son apparition sur la facture.
Vous pouvez ensuite remplir les résultats dans l’**Outil de dénombrement SA Azure Hybrid Benefit pour WindowsServer** ci-dessous pour obtenir le nombre de licences WS couvertes par le SA ou la souscription qui sont nécessaires.

Veillez à effectuer un inventaire de chaque souscription que vous détenez afin de générer une vue complète de votre positionnement de licence.

[Outil de dénombrement SA Azure Hybrid Benefit pour WindowsServer](http://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

Si vous avez effectué l’une des actions ci-dessus et confirmé que vous disposez de toutes les licences nécessaires pour le nombre d’instances Azure Hybrid Benefit que vous exécutez, vous n’avez rien d’autre à faire. Si vous avez découvert que vous pouvez couvrir davantage d’ordinateurs virtuels avec l’avantage, vous voudrez peut-être optimiser vos coûts en basculant les instances en cours d’exécution vers le coût avec avantage plutôt que le coût plein.

Si vous n’avez pas assez de licences WindowsServer éligibles pour le nombre d’ordinateurs virtuels déjà déployés, vous devez acheter des licences WindowsServer locales supplémentaires couvertes par Software Assurance via l’un des canaux indiqués ci-dessous, acheter des ordinateurs virtuels WindowsServer au tarif horaire standard ou désactiver la fonctionnalité Hybrid Benefit pour certains ordinateurs virtuels. Veuillez noter que vous pouvez acheter des licences de base par incrément de 8cœurs pour vous qualifier pour chaque ordinateur virtuel Azure Hybrid Benefit supplémentaire. 

SoftwareAssurance et/ou les souscriptions WindowsServersont disponibles à l’achat au travers des canaux d’émission de licence Microsoft suivants:

| Canal                      | Open     | OVS      | Sélectionner / Sélectionner plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| Taille standard (nombre de périphériques)  | 5-250    | 5-250    | > 250                  | > 250      | > 500     |
| SA / Souscription            | Facultatif | Inclus | Facultatif              | Facultatif  | Inclus |

Microsoft se réserve le droit d’auditer le client final à tout moment pour vérifier son éligibilité à l’utilisation d’Azure Hybrid Benefit. 

## Instructions de déploiement 

Nous avons mis des images de galerie prédéfinie à disposition de tous nos clients qui disposent de licences éligibles, indépendamment de l’endroit où ils en ont fait l’acquisition, et nous avons donné à nos partenaires la possibilité d’effectuer le déploiement pour le compte des clients. 

Vous trouverez des instructions pour toutes les options de déploiement disponibles [ici](https://azure.microsoft.com/pricing/hybrid-use-benefit/), notamment: 
-   une vidéo détaillée axée sur la nouvelle expérience de déploiement utilisant des images de la galerie prédéfinie;
-   des instructions détaillées sur le téléchargement d’un ordinateur virtuel personnalisé; 
-   des instructions détaillées sur la migration d’ordinateurs virtuels existants en utilisant Azure Site Recovery avec PowerShell. 
