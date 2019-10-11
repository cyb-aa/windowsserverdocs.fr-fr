---
title: Résolution des problèmes des clients ADBA
description: Décrit un processus de dépannage pour un problème d’activation de Windows
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b4e31cfa892019e4f3bbcd3b67dbb42751cc58dd
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963035"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>Exemple : Résolution des problèmes des clients avec activation basée sur Active Directory (ADBA, Active Directory Based Activation) qui ne s’activent pas

> [!NOTE]
> Cet article a été publié à l’origine en tant que billet de blog TechNet le 26 mars 2018.

Bonjour à tous ! Mon nom est Mike Kammer et je suis ingénieur « Platforms PFE » chez Microsoft depuis maintenant deux ans. J’ai récemment aidé un client à déployer Windows Server 2016 dans son environnement. Nous avons profité de cette occasion pour migrer également leur méthodologie d’activation depuis un serveur KMS vers l’[activation basée sur Active Directory](https://docs.microsoft.com/previous-versions/windows/hh852637(v=win.10)).

À titre de procédure appropriée pour apporter toutes les modifications, nous avons commencé notre migration dans l’environnement de test du client. Nous avons commencé notre déploiement en suivant les instructions de cet excellent billet de blog posté par Charity Shelbourne, [Active Directory-Based Activation vs. Key Management Services](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016). Les contrôleurs de domaine de notre environnement de test exécutaient tous Windows Server 2012 R2 : il n’était donc pas nécessaire de préparer notre forêt. Nous avons installé le rôle sur un contrôleur de domaine Windows Server 2012 R2 et nous avons choisi l’activation basée sur Active Directory comme méthode d’activation en volume. Nous avons installé notre clé KMS et lui avons donné le nom « KMS AD Activation ( ** LAB) ». Nous avons pratiquement suivi le billet de blog pas à pas.

Nous avons commencé par créer quatre machines virtuelles, deux Windows 2016 Standard et deux Windows 2016 Datacenter. À ce stade, tout était parfait et tout le monde était content. Nous avons créé un serveur physique exécutant Windows 2016 Standard, et la machine s’est activée correctement. C’est là que se termine notre histoire.

Et bien non ! Je plaisante ! Rien n’est jamais aussi facile. En vérité, l’installation et la configuration on été très faciles, et cette partie a été simple et directe. Je suis revenu au bureau le lundi, et toutes les machines virtuelles que j’avais créées la semaine précédente indiquaient qu’elles n’étaient pas activées. Curieux ! Ce n’est pas possible ! Je suis revenu voir la machine physique et tout allait bien. J’ai revu le client pour discuter de ce qui s’était produit. Bien entendu, la première question a été « Qu’est-ce qui a changé au cours du week-end ? ». Et comme d’habitude, la réponse a été « rien ». Cette fois-ci, réellement, rien n’avait été changé et nous avons dû déterminer ce qui s’était passé.

J’ai accédé à un de mes serveurs posant problème, j’ai ouvert une invite de commandes et j’ai vérifié la sortie de la commande **slmgr /ao-list**. Le commutateur **/ao-list** affiche tous les objets d’activation dans Active Directory.

![Image montrant la commande slmgr](./media/032618_1700_Troubleshoo1.png)

![Image montrant les résultats de la commande slmgr](./media/032618_1700_Troubleshoo2.png)

Les résultats montrent que nous avons deux objets d’activation : un pour le serveur sous Windows Server 2012 R2 et un pour notre activation « KMS AD Activation (** LAB) » nouvellement créée, qui est notre licence Windows Server 2016. Ceci confirme que notre annuaire Active Directory est correctement configuré pour activer des clients KMS Windows.

Sachant que la commande **slmgr** m’aide pour l’activation des licences, j’ai continué avec différentes options. J’ai essayé le commutateur **/dlv**, qui affiche des informations détaillées sur les licences. Cela m’a paru correct, j’exécutais la version Standard de Windows Server 2016, il existe un ID d’activation, un ID d’installation, une URL de validation et même une clé de produit partielle.

![Image montrant les résultats de la commande slmgr /dlv](./media/ActivationTroubleshoot2b.jpg)

Quelqu’un voit-il ce que j’ai raté à ce stade ? Nous y reviendrons après mes autres étapes de dépannage, mais on peut déjà dire que la réponse est dans cette capture d’écran.

Je pense maintenant que, pour une raison quelconque, la clé est endommagée : j’utilise donc le commutateur **/upk**, qui désinstalle la clé actuelle. Bien que cela ait fonctionné pour supprimer la clé, il ne s’agit généralement pas de la meilleure façon de le faire. Si le serveur devait être redémarré avant d’obtenir une nouvelle clé, cela laisserait le serveur en mauvais état. J’ai découvert que l’utilisation du commutateur **/ipk** (ce que je fais plus tard dans ma résolution des problèmes) remplace la clé existante et qu’il s’agit d’une démarche bien plus sûre. Apprenez de mes erreurs !

![Image montrant la commande slmgr /upk et son résultat](./media/032618_1700_Troubleshoo3.png)

J’ai réexécuté la commande avec le commutateur **/dlv** pour voir les informations détaillées de la licence. Malheureusement, cela ne m’a pas donné d’informations utiles, juste une erreur de clé de produit introuvable. En effet, il n’y a pas de clé puisque je viens de la désinstaller !

![Image montrant la commande slmgr /dlv et son résultat](./media/032618_1700_Troubleshoo4.png)

Au cas où, j’ai essayé le commutateur **/ato**, qui devrait activer Windows sur les serveurs KMS connus (ou Active Directory selon le cas). Là encore, j’ai obtenu seulement une erreur de produit introuvable.

![Image montrant la commande slmgr /ato et son résultat](./media/032618_1700_Troubleshoo5.png)

J’ai ensuite pensé que parfois, l’arrêt et le démarrage d’un service peuvent faire l’affaire : j’ai donc essayé cela. Je dois arrêter et démarrer le service Plateforme de protection logicielle Microsoft (SPPSvc). À partir d’une invite de commandes d’administration, j’utilise les fidèles commandes **net stop** et **net start**. Je remarque que le service n’est pas en cours d’exécution : je pense donc que ça doit être ça !

![Image montrant les résultats des commandes net stop et net start](./media/032618_1700_Troubleshoo6.png)

Mais non. Après le démarrage du service et une nouvelle tentative d’activation de Windows, j’obtiens toujours l’erreur de produit introuvable.

J’ai ensuite examiné le journal des événements d’application sur un des serveurs problématiques. Je trouve une erreur liée à l’activation de la licence, l’ID d’événement 8198, qui a un code 0x8007007B.

![Image montrant les détails de l’ID d’événement 8198](./media/032618_1700_Troubleshoo7.png)

En recherchant ce code, j’ai trouvé un article indiquant que mon code d’erreur signifie que la syntaxe du nom de fichier, du nom de répertoire ou du nom de volume est incorrecte. En lisant les méthodes décrites dans l’article, il m’a semblé qu’aucune d’entre elles n’était adaptée à ma situation. Quand j’ai exécuté la commande **nslookup -type=all _vlmcs._tcp**, j’ai trouvé le serveur KMS existant (il reste un grand nombre d’ordinateurs Windows 7 et Windows Server 2008 dans l’environnement, il était donc nécessaire de le conserver), mais également les cinq contrôleurs de domaine. Ceci indiquait qu’il ne s’agissait pas d’un problème de DNS et que mes problèmes étaient ailleurs.

![Image montrant les résultats de la commande nslookup](./media/032618_1700_Troubleshoo8.png)

Je sais ainsi que le DNS est correct. Active Directory est correctement configuré en tant que source d’activation KMS. Mon serveur physique a été activé correctement. Pourrait-il s’agir d’un problème concernant simplement les machines virtuelles ? À ce stade, mon client m’informe qu’une personne d’un autre département a décidé de créer plus d’une dizaine de machines virtuelles Windows Server 2016. Je suppose que je vais maintenant devoir m’occuper d’une autre dizaine de serveurs qui ne vont pas s’activer. Mais non ! Ces serveurs se sont parfaitement activés.

Bien, je suis revenu à ma commande **slmgr** pour déterminer comment faire en sorte que ces monstres soient activés. Cette fois-ci, je vais utiliser le commutateur **/ipk**, qui va me permettre d’installer une clé de produit. J’ai accédé à [ce site](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)) pour obtenir les clés appropriées pour ma version Standard de Windows Server 2016. Certains de mes serveurs sont sous Windows Server Datacenter, mais je dois d’abord résoudre ce problème-ci.

![Image montrant la liste des clés de configuration des clients KMS](./media/032618_1700_Troubleshoo9.png)

J’ai utilisé le commutateur **/ipk** pour installer une clé de produit, en choisissant la clé Windows Server 2016 Standard.

![Image montrant la commande slmgr /ipk et ses résultats](./media/032618_1700_Troubleshoo10.png)

À partir d’ici, j’ai capturé seulement les résultats de mes expériences pour l’édition Datacenter, mais ils étaient les mêmes. J’ai utilisé le commutateur **/ato** pour forcer l’activation. J’ai reçu un message agréable indiquant que le produit avait été activé avec succès !

![Image montrant la commande slmgr /ato et son résultat](./media/032618_1700_Troubleshoo11.png)

En utilisant à nouveau le commutateur **/dlv**, nous pouvons voir que nous avons maintenant été activés par Active Directory.

![Image montrant la commande slmgr /dlv et son résultat](./media/032618_1700_Troubleshoo12.png)

Alors, qu’est-ce qui s’est mal passé ? Pourquoi ai-je dû supprimer la clé installée et ajouter ces clés génériques pour que ces machines soient activées correctement ? Pourquoi l’autre dizaine de machines ont-elles été activées sans problème ? Comme je l’ai dit précédemment, j’ai manqué quelque chose dans les premières étapes de l’examen du problème. J’étais complètement déconcerté, j’ai donc contacté Charity, qui a écrit le billet de blog initial, pour voir si elle pouvait m’aider. Elle a vu tout de suite le problème et m’a aidé à comprendre ce que j’avais manqué au début.

Quand j’ai exécuté la première commande avec le commutateur **/dlv**, la clé était dans la description. La description était « Système d’exploitation Windows®, Canal RETAIL ». J’ai examiné cela et j’ai pensé que le canal RETAIL signifiait qu’il avait été acheté et que c’était une clé valide.

![Image montrant les résultats de la commande slmgr /dlv, avec les informations du canal mises en évidence](./media/032618_1700_Troubleshoo13.png)

Quand nous examinons la sortie du commutateur **/dlv** à partir d’un serveur correctement activé, vous remarquez que la description indique maintenant le canal VOLUME_KMSCLIENT. Ceci nous permet de savoir qu’il s’agit effectivement d’une licence en volume.

![Image montrant les résultats de la commande slmgr /dlv, avec les informations du canal mises en évidence](./media/032618_1700_Troubleshoo14.png)

Que signifie alors le canal RETAIL ? En fait, cela signifie que le média utilisé pour installer le système d’exploitation était une image ISO MSDN. Je suis revenu vers mon client et j’ai demandé si par hasard, une image ISO de Windows Server 2016 se trouvait quelque part sur le réseau. Il s’est avéré que oui, il existait bien une autre image ISO sur le réseau, et qu’elle avait été utilisée pour créer l’autre dizaine de machines. Ils ont comparé les deux images ISO et il est apparu que celle qui m’avait été donnée pour créer les serveurs virtuels était en fait une image ISO MSDN. Ils ont supprimé cette image ISO MSDN de leur réseau et nous avons maintenant tous nos serveurs existants activés, et nous n’avons plus d’inquiétudes quant aux échecs d’activation sur les futures créations de machines.

J’espère que ceci vous a été utile et peut vous faire gagner du temps !

Mike
