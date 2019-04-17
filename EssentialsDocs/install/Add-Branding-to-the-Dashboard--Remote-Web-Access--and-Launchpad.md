---
title: "Ajout d’un logo au tableau de bord, accès Web à distance et Launchpad"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Ajout d’un logo au tableau de bord, accès Web à distance et Launchpad

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_Branding"></a>Ajout d’un logo au tableau de bord, accès Web à distance et Launchpad  
 Vous pouvez compléter nombreux marque en ajoutant des entrées dans le Registre. Toutes les entrées dans le Registre pour le système d’exploitation sont localisées sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM.  
  
 Tous les éléments de personnalisation doivent respecter les spécifications suivantes:  
  
-   Le logo WindowsServerEssentials doit avoir une largeur minimale de **170pixels**, conserver une proportion correcte, avec **96PPP**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Pour ajouter la personnalisation en modifiant le Registre  
  
1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur le **Regedit** application.  
  
3.  Dans le volet de navigation, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**. Si le **OEM** clé n’existe pas, procédez comme suit pour la créer:  
  
    1.  Avec le bouton droit **Windows Server**, cliquez sur **New**, puis cliquez sur **clé**.  
  
    2.  Type **OEM** pour le nom de la clé.  
  
4.  (Facultatif) Si vous créez une entrée pour un logo, vous pouvez créer des clés différentes pour différencier les versions de langue du logo. Par exemple, si vous avez les versions anglaise et allemandes d’un logo, vous pouvez créer un en-us de clé et une clé fr-fr. Étant donné que tous les fichiers des logos sont stockés dans le même dossier, vous devez fournir les instances du fichier image du logo avec un nom unique pour chaque langue. Par exemple, vous feriez créer un fichier appelé DashboardLogo_en.png et DashboardLogo_de.png.  
  
5.  Un clic droit **OEM** ou avec le bouton droit de la clé de langue approprié, cliquez sur **New**, puis cliquez sur **valeur de chaîne**.  
  

6.  Entrez le nom de la chaîne, puis appuyez sur ENTRÉE. Reportez-vous à la [Registre chaînes et valeurs](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) table pour les valeurs de noms et les données de chaîne.  

6.  Entrez le nom de la chaîne, puis appuyez sur ENTRÉE. Reportez-vous à la [Registre chaînes et valeurs](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) table pour les valeurs de noms et les données de chaîne.  

  
7.  Avec le bouton droit de la nouvelle chaîne, puis cliquez sur **modifier**.  
  
8.  Entrez la valeur de la table qui est associée au nom de chaîne, puis cliquez sur **OK**.  
  
9. Si vous créez une entrée pour une image du logo ou de des liens ajoutés, copiez le fichier dans %programFiles%\Windows Server\Bin\OEM. Si le répertoire OEM n’existe pas, créez-le.  
  
10. Si des modifications qui affectent l’accès Web à distance, une fois que le client prenne possession du serveur, ils doivent activer accès Web à distance. Informez le client pour effectuer les opérations suivantes:  
  
    1.  Dans le tableau de bord, cliquez sur **paramètres**, puis cliquez sur le **accès en tout lieu** onglet.  
  
    2.  Si l’accès en tout lieu est activé, cliquez sur **configurer**, puis désactivez case à cocher de l’accès Web à distance sur le **fonctionnalités choisir accès en tout lieu à activer** page de l’Assistant accès en tout lieu configurer.  
  
    3.  Cliquez sur **configurer**.  
  
###  <a name="BKMK_RegStrings"></a>Le tableau suivant répertorie l’emplacement où les modifications apportées au Registre affectent personnalisation, le nom de chaîne et la valeur de données.  
  
### <a name="registry-strings-and-values"></a>Les valeurs et les chaînes de Registre  
  
|Emplacement de personnalisation|Description|Nom de chaîne|Valeur de données|  
|--------------------------|-----------------|-----------------|----------------|  
|Logo du tableau de bord|Ajoute l’image du logo au tableau de bord. Le logo du tableau de bord doit être au format .png et ne doit pas être supérieur à 350pixels de large par 38pixels de haut.<br /><br /> **Important:** pour donner le tableau de bord avec votre logo, vous devez modifier la vignette de l’illustration qui est fournie sur le DVD du Kit de préinstallation OEM et ajoute le logo de votre société à l’image, en suivant les exigences de l’espace blanc approprié. Pour plus d’informations, consultez la vignette de l’exemple fourni.|DashboardLogo|Nom du fichier image du logo|  
|DashboardClientLogo|Ajoute l’image du logo à l’écran de connexion du client du tableau de bord.|DashboardClientLogo|Nom du fichier image du logo|  
|Image d’arrière-plan du site Web|Modifie l’image d’arrière-plan qui s’affiche sur la page d’ouverture de session accès Web à distance. Résolutions courantes apparaîtra comme suit:<br /><br /> -1024 x 768pixels la résolution recouvre la page d’ouverture de session<br /><br /> -800 x 600pixels sera centré sur la page et apparaissent avec une bordure noire<br /><br /> -1280 x 720pixels la résolution sera centré et n’apparaissent pas les pixels qui dépassent 1024 x 768.|LogonBackground|Nom du fichier image d’arrière-plan|  
|Titre du site Web|Remplace le titre du site accès Web à distance de WindowsServerEssentials à un titre que vous choisissez.|Nom du site Web|Nouveau titre du site accès Web à distance|  
|Logo du site Web|Modifie le logo par défaut sur le site accès Web à distance. La taille attendue du logo est de 32pixels par 32pixels. Si votre logo sont inférieures ou supérieures, elle sera agrandie ou réduite en conséquence les dimensions|WebsiteLogo|Nom du fichier image du logo|  
|Ajouté logo du site Web|Logo de partenaire affiche juste en dessous du logo Microsoft qui s’affiche sur le site accès Web à distance. La taille attendue du logo est de 200pixels de haut par 50pixels de large. Si votre logo est plus grand, il sera automatiquement réduit pour s’adapter à tout en préservant les proportions d’origine. Si votre logo est plus petit, il sera centré dans l’espace de pixel 200 x 50 et la taille, ni les proportions sont modifiées.|OEMLogo|Nom du fichier image du logo|  

| Liens sur la page d’accueil et de la page d’ouverture de session | Ajouter des liens à la page d’ouverture de session et de la page d’accueil du site accès Web à distance. Le fichier .xml contenant les informations de liaison doit se trouver dans %programFiles%\Windows Server\Bin\OEM. L’exemple suivant illustre le format du fichier .xml:<br /><br /> < oemLinks\ ><br /> < logonLinks\ ><br /> < lier nom\ = LogonLinkName ><br /> LogonLinkDescription < texte > < / texte ><br /> LogonLinkURL < url\ > < / Url\ ><br /> LinkIcon < icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < homepageLinks\ ><br /> < lier nom\ = HomepageLinkName ><br /> HomepageLinkDescription < texte > < / texte ><br /> HomepageLinkURL < url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Reportez-vous à la [éléments LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) table pour obtenir la liste des éléments et des descriptions. |  

| Liens sur la page d’accueil et de la page d’ouverture de session | Ajouter des liens à la page d’ouverture de session et de la page d’accueil du site accès Web à distance. Le fichier .xml contenant les informations de liaison doit se trouver dans %programFiles%\Windows Server\Bin\OEM. L’exemple suivant illustre le format du fichier .xml:<br /><br /> < oemLinks\ ><br /> < logonLinks\ ><br /> < lier nom\ = LogonLinkName ><br /> LogonLinkDescription < texte > < / texte ><br /> LogonLinkURL < url\ > < / Url\ ><br /> LinkIcon < icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < homepageLinks\ ><br /> < lier nom\ = HomepageLinkName ><br /> HomepageLinkDescription < texte > < / texte ><br /> HomepageLinkURL < url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Reportez-vous à la [éléments LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) table pour obtenir la liste des éléments et des descriptions. |  

| Logo LaunchPad | Ajoute l’image du logo au Launchpad. Le logo du Launchpad doit être au format .png et de doit pas dépasser 64pixels. | LaunchpadLogo | Nom du fichier image du logo |  
  
###  <a name="BKMK_Links"></a>Le tableau suivant répertorie et décrit les éléments de nom de chaîne LinksXML.  
  
### <a name="linksxml-elements"></a>Éléments LinksXML  
  
|Élément LinksXML|Description|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|Le nom du lien d’ouverture de session.|  
|LogonLinkDescription|Le texte qui est affiché sous forme de lien de page d’ouverture de session.|  
|LogonLinkURL|L’url permettant de résoudre le lien de la page d’ouverture de session.|  
|LinkIcon|Le nom du fichier icône pour le lien d’ouverture de session. Ce fichier doit être dans le même dossier que le fichier .xml. Images d’icônes de lien doivent mesurer 16 x 16pixels et être au format .png. Si vous ne spécifiez pas un LinkIcon, l’image d’icône de lien par défaut est utilisé.|  
|**HomepageLinks**|  
|HomepageLinkName|Le nom du lien page d’accueil.|  
|HomepageLinkDescription|Le texte qui apparaît sous forme de lien de page d’accueil.|  
|HomepageLinkURL|L’URL permettant de résoudre le lien de page d’accueil.|  
|HomepageLinkIcon|Le nom du fichier icône pour le lien de page d’accueil. Ce fichier doit être dans le même dossier que le fichier .xml. Images HomepageLinkIcon doivent mesurer 16 x 16pixels et être au format .png. Si vous ne spécifiez pas un HomepageLinkIcon, l’image représentant le lien page d’accueil par défaut est utilisé.|  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

