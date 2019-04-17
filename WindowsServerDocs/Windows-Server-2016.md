---
redirect_url: /windows-server/windows-server
ms.openlocfilehash: aa1bc1d94f91a2b9584f72398385575d22db33a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="windows-server-2016"></a>Windows Server2016

Cette bibliothèque fournit des informations permettant aux professionnels de l’informatique d'évaluer, planifier, déployer, sécuriser et gérer Windows Server2016.

> [!Note] 
> La prochaine version de Windows Server change! Vous trouverez plus d’informations sur ce qui se prépare en vous rendant sur [Présentation du canal semi-annuel de Windows Server](./get-started/semi-annual-channel-overview.md). 

[![WPrésentation vidéo de Windows Server2016](media/front-page-video.png)](https://www.youtube.com/embed/V8oF0JpDzaM)

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/what-s-new-in-windows-server-2016">
        <img height=145 src="media/whats-new-highlight.png" alt="What's new icon" title="Nouveautés de WindowsServer2016"/></a>
        <br/>Nouveautés
    </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/server-basics">
        <img height=145 src="media/1-getstarted.png" alt="get started icon" title="Prise en main de Windows Server2016" /></a>
      <br/>Prise en main </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/administration/index">
        <img height=145 src="media/8-management.png" alt="administer icon" title="Administrer Windows Server" /></a>
      <br/>Administrer </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/failover-clustering/failover-clustering-overview">
        <img height=145 src="media/3-failover.png" alt="Failover clustering icon" title="Clustering de basculement Windows Server" /></a>
      <br/>Clustering de basculement </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/identity/identity-and-access">
        <img height=145 src="media/4-identity.png" alt="Identity and access icon" title="Identité et accès Windows Server" /></a>
      <br>Identité et accès </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/networking/networking">
        <img height=145 src="media/6-networking.png" alt="Networking icon" title="Mise en réseau de WindowsServer" />
        </a>
      <br/>Réseaux </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/remote/index">
        <img height=145 src="media/remote.png" alt="remote icon" title="Accès à distance et gestion de serveur" />
        </a>
      <br/>Accès à distance </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/security/security-and-assurance">
        <img height=145 src="media/5-security.png" alt="Security icon" title="Sécurité et assurance Windows Server" />
      </a>
      <br/>Sécurité et assurance </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">&nbsp;</td>
    <td align='center' style="width:25%; border:0;"><br>
      <a href="/windows-server/storage/storage">
        <img height=145 src="media/7-storage.png" alt="Storage icon" title="Stockage Windows Server" />
      </a>
      <br/>Stockage </td>
   <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/virtualization/virtualization">
        <img height=145 src="media/virtualization.png" alt="virtualization icon" title="Virtualisation Windows Server" /></a>
      <br/>Virtualisation </td>
    <td align='center' style="width:25%; border:0;">&nbsp; </td>
  </tr>
</table>

<br/>

> [!Note] 
> Pour découvrir les nouvelles fonctionnalités disponibles dans WindowsServer2016, vous pouvez télécharger une version d’évaluation à partir de la page [Windows Server-Évaluations](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). 


## <a name="windows-server-2016-editions"></a>Éditions de WindowsServer2016

Windows Server2016 est disponible dans les éditions Standard, Datacenter et Essentials. Windows Server2016 Datacenter inclut des droits de virtualisation illimités et de nouvelles fonctionnalités permettant de créer un centre de données à définition logicielle. Windows Server2016 Standard offre des fonctionnalités d’entreprise avec des droits de virtualisation limitée. WindowsServerEssentials est une solution idéale comme premier serveur connecté au cloud. Elle possède une [documentation complète](http://go.microsoft.com/fwlink/?LinkID=827171); toutefois, le contenu présenté ici ne concerne que les éditionsStandard et Datacenter. Le tableau suivant résume les principales différences entre les éditionsStandard et Datacenter:

|Fonctionnalité|Datacenter|Standard|  
|-------------------|----------|-----------------------|  
|Fonctionnalités principales de WindowsServer| oui| oui|
|Systèmes d’exploitation / Conteneurs Hyper-V|Illimité|   2|
|Conteneurs Windows Server|Illimité|   Illimité|
|Service Guardian hôte| oui| oui|
|Option d’installation de NanoServer| oui| oui|
|Fonctionnalités de stockage, y compris les espaces de stockage direct et le réplica de stockage| oui| non|
|Machines virtuelles dotées d’une protection maximale| oui| non|
|Infrastructure de mise en réseau SDN (contrôleur de réseau, équilibrage de charge logicielle et passerelle multi-locataire)| oui| non|

Pour plus d’informations, voir [Pricing and licensing for Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server-pricing) (Tarifs et licences de Windows Server2016) et [Compare features in Windows Server versions](https://www.microsoft.com/en-us/cloud-platform/windows-server-comparison) (Comparaison des fonctionnalités des versions de Windows Server).

## <a name="installation-options"></a>Options d’installation

Les éditionsStandard et Datacenter proposent troisoptions d’installation:

- **Server Core** (Installation minimale): réduit l’espace nécessaire sur le disque, la surface d’attaque potentielle et, surtout, les besoins en maintenance. Cette option est **recommandée**, sauf si vous avez besoin d’autres éléments d’interface utilisateur et outils de gestion graphiques.
- **Server with Desktop Experience** (Serveur avec expérience utilisateur): installe l’interface utilisateur standard et tous les outils, notamment les fonctionnalités d’expérience client nécessitant une installation distincte dans Windows Server2012R2. Les rôles serveur et les fonctionnalités sont installés à l’aide du Gestionnaire de serveur ou d’autres méthodes.
- **Nano Server**: système d’exploitation de serveur administré à distance et optimisé pour les centres de données et clouds privés. Il est similaire à Windows Server en mode ServerCore (Installation minimale), mais il est beaucoup plus petit, n’a aucune fonction d’ouverture de session locale et ne prend en charge que les agents, outils et applications 64bits. Il occupe beaucoup moins d’espace disque, s’installe nettement plus rapidement et nécessite beaucoup moins de mises à jour et de redémarrages que les autres options.

>[!Note]
> À la différence des versions précédentes de Windows Server, vous ne pouvez pas basculer entre l’installation minimale et l’installation avec Expérience utilisateur après l’installation. Par exemple, si vous procédez à l’installation minimale et décidez ultérieurement d’utiliser l’installation avec Expérience utilisateur, vous devez procéder à une nouvelle installation (et inversement).


Maintenant que vous savez quelle édition et option d’installation vous conviennent, cliquez ci-dessous pour prendre en main WindowsServer2016.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:33%; border:0;">
      <a  href="/windows-server/get-started/getting-started-with-nano-server"> <img width="175" src="media/nano.png" alt="Icon representing Nano server" title="Nano Server - Poids le plus faible" /><br/>Nano Server - <br/>Poids le plus faible</a>
    </td>
    <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-core"> <img width="175" src="media/servercore.png" alt="Icon representing the Server Core installation" title="Server Core - recommandé" /><br/>Server Core - <br/>Recommandé</a></td>
   <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-with-desktop-experience"><img width="175" src="media/desktop.png" alt="Icon representing the full desktop experience installation option for Windows Server" title="Ordinateur de bureau - Expérience utilisateur complète" /><br/>Expérience utilisateur pour ordinateur de bureau - <br/>Interface complète</a></td>
  </tr>
</table>

## <a name="windows-server-software-defined-datacenter-sddc"></a>Centre de données défini par logiciel WindowsServer (SDDC)

Les technologies de stockage, de mise en réseau, de sécurité et de gestion virtualisées sont les blocs de construction d’un centre de données défini par logiciel (SDDC) WindowsServer.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:10%; border:0;"></td>
    <td align='center' style="width:50%; border:0;"><a href="/windows-server/sddc"><img width="400" src="media/sddc/WS16-heading.png" alt="Icon representing SDDC" title="Centre de données défini par logiciel WindowsServer (SDDC)" /><br/>Centre de données défini par logiciel WindowsServer (SDDC)</a></td>
    <td align='center' style="width:10%; border:0;"></td>
  </tr>
</table>

Vous ne trouvez pas ce dont vous avez besoin? Utilisateurs de Windows10, dites-nous ce que vous recherchez dans le [Hub de commentaires](feedback-hub://?referrer=techDocsUcPage&tabid=2&contextid=898&newFeedback=true&topic=Windows-Server-2016.md). 
