---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: "Annexe L - événements à surveiller"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d153a16b31070f2bbfac4a47a814792ef66b472
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-l-events-to-monitor"></a>Annexel: Événements à surveiller

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

  
## <a name="appendix-l-events-to-monitor"></a>Annexel: Événements à surveiller  
Le tableau suivant répertorie les événements que vous devez surveiller dans votre environnement, selon les recommandations fournies dans [analyse ActiveDirectory pour les signes de compromission](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Dans le tableau suivant, la colonne «ID d’événement Windows actuel» répertorie l’ID d’événement tel qu’il est implémenté dans les versions de Windows et Windows Server qui se trouvent actuellement dans le support standard.  
  
La colonne «ID d’événement Windows hérités» répertorie l’ID d’événement correspondant dans les versions héritées de Windows tels que les ordinateurs clients exécutant WindowsXP ou version antérieure et les serveurs exécutant Windows Server2003 ou version antérieure. La colonne «importance potentielle» colonne identifie si l’événement doit être considérée comme de faible, moyenne ou grande importance dans la détection des attaques et la colonne «Synthèse de l’événement» fournit une brève description de l’événement.  
  
Une importance potentielle élevée signifie qu’une occurrence de l’événement doit être traité. Importance potentielle moyenne ou faible signifie que ces événements doivent seulement être analysés s’ils se produisent ou de façon inattendue en nombre qui dépasse considérablement la ligne de base attendu dans un laps de temps mesuré. Toutes les organisations doivent tester ces recommandations dans leurs environnements avant de créer des alertes qui requièrent obligatoirement une investigation. Chaque environnement est différent, et certains événements classés avec une importance potentielle élevée peuvent survenir en raison d’autres événements inoffensifs.  
  
|||||  
|-|-|-|-|  
|**ID d’événement Windows actuel**|**ID d’événement Windows hérités**|**Importance potentielle**|**Résumé des événements**|  
|4618|NON APPLICABLE|Élevé|Un modèle d’événement de sécurité analysé s’est produite.|  
|4649|NON APPLICABLE|Élevé|Attaques par relecture a été détecté. Peut être des faux positifs en raison d’erreurs d’une configuration incorrecte.|  
|4719|612|Élevé|Stratégie d’audit système a été modifié.|  
|4765|NON APPLICABLE|Élevé|SID History a été ajouté à un compte.|  
|4766|NON APPLICABLE|Élevé|Échec d’une tentative pour ajouter des SID History à un compte.|  
|4794|NON APPLICABLE|Élevé|Une tentative a été effectuée pour définir le Mode de restauration des Services de répertoire.|  
|4897|801|Élevé|Séparation de rôle activée:|  
|4964|NON APPLICABLE|Élevé|Groupes spéciaux ont été attribuées à une nouvelle ouverture de session.|  
|5124|NON APPLICABLE|Élevé|Un paramètre de sécurité a été mis à jour sur le Service de répondeurs OCSP|  
|NON APPLICABLE|550|Moyenne à haute|Attaque possible attaque par déni de service (DoS)|  
|1102|517|Moyenne à haute|Le journal d’audit a été désactivé.|  
|4621|NON APPLICABLE|Moyenne|L’administrateur a récupéré le système à partir de CrashOnAuditFail. Les utilisateurs qui ne sont pas administrateurs pourront désormais ouvrir une session. Audit d’une activité ne peut pas ont été enregistrée.|  
|4675|NON APPLICABLE|Moyenne|SID ont été filtrés.|  
|4692|NON APPLICABLE|Moyenne|Sauvegarde de la clé principale de protection des données a été tentée.|  
|4693|NON APPLICABLE|Moyenne|Récupération de clé principale de protection des données a été tentée.|  
|4706|610|Moyenne|Une nouvelle approbation a été créée à un domaine.|  
|4713|617|Moyenne|Stratégie Kerberos a été modifié.|  
|4714|618|Moyenne|Stratégie de récupération a été modifié.|  
|4715|NON APPLICABLE|Moyenne|La stratégie d’audit (SACL) sur un objet a été modifiée.|  
|4716|620|Moyenne|Informations de domaine approuvé a été modifiées.|  
|4724|628|Moyenne|Une tentative de réinitialisation de mot de passe d’un compte.|  
|4727|631|Moyenne|Un groupe global de sécurité activée a été créé.|  
|4735|639|Moyenne|Un groupe local de sécurité activée a été modifié.|  
|4737|641|Moyenne|Un groupe global de sécurité activée a été modifié.|  
|4739|643|Moyenne|Stratégie de domaine a été modifié.|  
|4754|658|Moyenne|Un groupe universel de sécurité activée a été créé.|  
|4755|659|Moyenne|Un groupe universel de sécurité activée a été modifié.|  
|4764|667|Moyenne|Un groupe de sécurité est désactivée a été supprimé.|  
|4764|668|Moyenne|Type de groupe a été modifié.|  
|4780|684|Moyenne|La liste ACL a été définie sur les comptes qui sont membres du groupe Administrateurs.|  
|4816|NON APPLICABLE|Moyenne|RPC a détecté une violation d’intégrité lors du déchiffrement d’un message entrant.|  
|4865|NON APPLICABLE|Moyenne|Une entrée d’informations de forêt approuvée a été ajoutée.|  
|4866|NON APPLICABLE|Moyenne|Une entrée d’informations de forêt approuvée a été supprimée.|  
|4867|NON APPLICABLE|Moyenne|Une entrée d’informations de forêt approuvée a été modifiée.|  
|4868|772|Moyenne|Le Gestionnaire de certificats a refusé une demande de certificat en attente.|  
|4870|774|Moyenne|Services de certificats ont révoqué un certificat.|  
|4882|786|Moyenne|Les autorisations de sécurité pour les Services de certificats modifiés.|  
|4885|789|Moyenne|Le filtre d’audit pour les Services de certificats modifiés.|  
|4890|794|Moyenne|Les paramètres du Gestionnaire de Services de certificats modifiés.|  
|4892|796|Moyenne|Une propriété des Services de certificats modifiés.|  
|4896|800|Moyenne|Une ou plusieurs lignes ont été supprimés de la base de données du certificat.|  
|4906|NON APPLICABLE|Moyenne|La valeur de CrashOnAuditFail a changé.|  
|4907|NON APPLICABLE|Moyenne|Paramètres d’audit sur l’objet ont été modifiées.|  
|4908|NON APPLICABLE|Moyenne|Table des groupes d’ouverture de session spéciale modifié.|  
|4912|807|Moyenne|Par la stratégie d’Audit de l’utilisateur a été modifié.|  
|4960|NON APPLICABLE|Moyenne|IPsec a supprimé un paquet entrant qui n’a pas pu une vérification d’intégrité. Si le problème persiste, cela peut indiquer qu'un problème réseau ou qui les paquets sont modifiés en transit vers cet ordinateur. Vérifiez que les paquets envoyés à partir de l’ordinateur distant sont les mêmes que celles reçu par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations d’IPsec.|  
|4961|NON APPLICABLE|Moyenne|IPsec a supprimé un paquet entrant qui n’a pas pu une vérification de la relecture. Si le problème persiste, cela peut indiquer une attaque par rapport à cet ordinateur.|  
|4962|NON APPLICABLE|Moyenne|IPsec a supprimé un paquet entrant qui n’a pas pu une vérification de la relecture. Le paquet entrant a trop faible un numéro de séquence pour vous assurer qu’il n’a pas une relecture.|  
|4963|NON APPLICABLE|Moyenne|IPsec a supprimé un paquet entrant clair qui devrait être sécurisé. Il s’agit généralement en raison de l’ordinateur distant, modification de la stratégie IPsec sans informer de cet ordinateur. Cela peut également être une tentative d’attaque par usurpation d’identité.|  
|4965|NON APPLICABLE|Moyenne|IPsec a reçu un paquet à partir d’un ordinateur distant avec l’Index de paramètre de sécurité (SPI) un incorrect. Cela est généralement dû à un matériel défectueux qui endommage les paquets. Si ces erreurs persistent, vérifiez que les paquets envoyés à partir de l’ordinateur distant sont les mêmes que celles reçu par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations d’IPsec. Dans ce cas, si la connectivité n’est pas entravée, puis ces événements peuvent être ignorés.|  
|4976|NON APPLICABLE|Moyenne|Lors de la négociation de Mode principal IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou de relire cette négociation.|  
|4977|NON APPLICABLE|Moyenne|Lors de la négociation de Mode rapide IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou de relire cette négociation.|  
|4978|NON APPLICABLE|Moyenne|Lors de la négociation de Mode étendu IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou de relire cette négociation.|  
|4983|NON APPLICABLE|Moyenne|Échec d’une négociation IPsec Extended Mode. L’association de sécurité en Mode principal correspondante a été supprimée.|  
|4984|NON APPLICABLE|Moyenne|Échec d’une négociation IPsec Extended Mode. L’association de sécurité en Mode principal correspondante a été supprimée.|  
|5027|NON APPLICABLE|Moyenne|Le Service pare-feu Windows n’a pas pu récupérer la stratégie de sécurité du stockage local. Le service va continuer à appliquer la stratégie actuelle.|  
|5028|NON APPLICABLE|Moyenne|Le Service pare-feu Windows n’a pas pu analyser la nouvelle stratégie de sécurité. Le service va continuer avec la stratégie en vigueur.|  
|5029|NON APPLICABLE|Moyenne|Le Service pare-feu Windows n’a pas pu initialiser le pilote. Le service continue à appliquer la stratégie actuelle.|  
|5030|NON APPLICABLE|Moyenne|Le Service pare-feu Windows n’a pas pu démarrer.|  
|5035|NON APPLICABLE|Moyenne|Le pilote du pare-feu Windows n’a pas pu démarrer.|  
|5037|NON APPLICABLE|Moyenne|Le pilote du pare-feu Windows a détecté l’erreur d’exécution critique. Fin d’exécution.|  
|5038|NON APPLICABLE|Moyenne|L’intégrité du code a déterminé que le hachage de l’image d’un fichier n’est pas valide. Le fichier peut être endommagé en raison de modifications non autorisées ou le hachage non valide peut indiquer une erreur de périphérique de disque potentielle.|  
|5120|NON APPLICABLE|Moyenne|Service de répondeurs OCSP a démarré|  
|5121|NON APPLICABLE|Moyenne|Arrêté du Service de répondeurs OCSP|  
|5122|NON APPLICABLE|Moyenne|Une entrée de configuration a changé dans le Service de répondeurs OCSP|  
|5123|NON APPLICABLE|Moyenne|Une entrée de configuration a changé dans le Service de répondeurs OCSP|  
|5376|NON APPLICABLE|Moyenne|Informations d’identification du Gestionnaire d’informations d’identification ont été sauvegardées.|  
|5377|NON APPLICABLE|Moyenne|Informations d’identification du Gestionnaire d’informations d’identification ont été restaurées à partir d’une sauvegarde.|  
|5453|NON APPLICABLE|Moyenne|Une négociation IPsec avec un ordinateur distant a échoué car le service IKE et AuthIP IPsec masquage Modules (IKEEXT) n’est pas démarré.|  
|5480|NON APPLICABLE|Moyenne|Les Services IPsec n’a pas pu obtenir la liste complète des interfaces réseau sur l’ordinateur. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peut ne pas Obtient la protection fournie par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5483|NON APPLICABLE|Moyenne|Les Services IPsec n’a pas pu initialiser le serveur RPC. Les Services IPsec n’a pas pu être démarrés.|  
|5484|NON APPLICABLE|Moyenne|IPsec Services a rencontré une défaillance critique et a été arrêté. L’arrêt des Services IPsec peut mettre l’ordinateur à un plus grand risque d’attaque de réseau ou exposer l’ordinateur à des risques de sécurité.|  
|5485|NON APPLICABLE|Moyenne|Les Services IPsec n’a pas pu traiter certains filtres IPsec sur un événement plug-and-play pour les interfaces réseau. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peut ne pas Obtient la protection fournie par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|6145|NON APPLICABLE|Moyenne|Une ou plusieurs erreurs s’est produite lors du traitement de stratégie de sécurité dans les objets de stratégie de groupe.|  
|6273|NON APPLICABLE|Moyenne|Serveur de stratégie réseau refusé l’accès à un utilisateur.|  
|6274|NON APPLICABLE|Moyenne|Serveur de stratégie réseau rejeté la demande d’un utilisateur.|  
|6275|NON APPLICABLE|Moyenne|Serveur de stratégie réseau rejeté la demande de gestion des comptes pour un utilisateur.|  
|6276|NON APPLICABLE|Moyenne|Serveur de stratégie réseau mis en quarantaine d’un utilisateur.|  
|6277|NON APPLICABLE|Moyenne|Serveur de stratégie réseau autorisés à accéder à un utilisateur mais à suspendue car l’ordinateur hôte ne satisfaisant pas la stratégie d’intégrité définie.|  
|6278|NON APPLICABLE|Moyenne|Serveur de stratégie réseau accordé un accès complet à un utilisateur, car l’hôte remplies la stratégie d’intégrité définie.|  
|6279|NON APPLICABLE|Moyenne|Serveur de stratégie réseau verrouillé le compte d’utilisateur en raison de tentatives répétées d’authentification.|  
|6280|NON APPLICABLE|Moyenne|Serveur de stratégie réseau déverrouillé le compte d’utilisateur.|  
|-|640|Moyenne|Base de données général compte modifié|  
|-|619|Moyenne|La qualité de Service stratégie modifié|  
|24586|NON APPLICABLE|Moyenne|Une erreur s’est produite conversion du volume|  
|24592|NON APPLICABLE|Moyenne|Une tentative de redémarrage automatique de conversion sur le volume %2 a échoué.|  
|24593|NON APPLICABLE|Moyenne|Écriture de métadonnées: renvoie des erreurs Volume %2lors de la tentative de modifier les métadonnées. Si les échecs continuent, déchiffrez le volume|  
|24594|NON APPLICABLE|Moyenne|Reconstruction de métadonnées: une tentative d’écriture d’une copie des métadonnées sur le volume %2 a échoué et peuvent apparaître sous la forme d’endommagement de disque. Si les échecs continuent, déchiffrez le volume.|  
|4608|512|Faible|Démarrage de Windows.|  
|4609|513|Faible|Arrêt de Windows.|  
|4610|514|Faible|Un package d’authentification a été chargé par l’autorité de sécurité locale.|  
|4611|515|Faible|Un processus d’ouverture de session a été enregistré avec l’autorité de sécurité locale.|  
|4612|516|Faible|Les ressources internes allouées pour la file d’attente des messages d’audit ont été épuisées, entraînant la perte de certaines des audits.|  
|4614|518|Faible|Un package de notification a été chargé par le Gestionnaire de comptes de sécurité.|  
|4615|519|Faible|Utilisation incorrecte de port LPC.|  
|4616|520|Faible|L’heure système a été modifié.|  
|4622|NON APPLICABLE|Faible|Un package de sécurité a été chargé par l’autorité de sécurité locale.|  
|4624|528,540|Faible|Un compte a été correctement connecté.|  
|4625|529-537,539|Faible|Un compte n’a pas pu ouvrir une session.|  
|4634|538|Faible|Un compte a été déconnecté.|  
|4646|NON APPLICABLE|Faible|Mode de prévention DoS IKE démarré.|  
|4647|551|Faible|Fermeture de session initiée par l’utilisateur.|  
|4648|552|Faible|Une ouverture de session a été lancée à l’aide des informations d’identification explicites.|  
|4650|NON APPLICABLE|Faible|Une association de sécurité de Mode principal IPsec a été établie. En Mode étendu n’était pas activé. L’authentification par certificat n’a pas été utilisée.|  
|4651|NON APPLICABLE|Faible|Une association de sécurité de Mode principal IPsec a été établie. En Mode étendu n’était pas activé. Un certificat a été utilisé pour l’authentification.|  
|4652|NON APPLICABLE|Faible|Échec d’une négociation de Mode principal IPsec.|  
|4653|NON APPLICABLE|Faible|Échec d’une négociation de Mode principal IPsec.|  
|4654|NON APPLICABLE|Faible|Échec d’une négociation en Mode rapide IPsec.|  
|4655|NON APPLICABLE|Faible|Une association de sécurité de Mode principal IPsec a pris fin.|  
|4656|560|Faible|Un handle vers un objet a été demandé.|  
|4657|567|Faible|Une valeur de Registre a été modifiée.|  
|4658|562|Faible|Le handle vers un objet a été fermé.|  
|4659|NON APPLICABLE|Faible|Un handle vers un objet a été demandé avec l’intention de supprimer.|  
|4660|564|Faible|Un objet a été supprimé.|  
|4661|565|Faible|Un handle vers un objet a été demandé.|  
|4662|566|Faible|Une opération a été effectuée sur un objet.|  
|4663|567|Faible|Une tentative d’accès à un objet.|  
|4664|NON APPLICABLE|Faible|Une tentative a été effectuée pour créer un lien physique.|  
|4665|NON APPLICABLE|Faible|Une tentative a été effectuée pour créer un contexte d’application client.|  
|4666|NON APPLICABLE|Faible|Une application a tenté d’une opération:|  
|4667|NON APPLICABLE|Faible|Un contexte d’application client a été supprimé.|  
|4668|NON APPLICABLE|Faible|Une application a été initialisée.|  
|4670|NON APPLICABLE|Faible|Les autorisations sur un objet ont été modifiées.|  
|4671|NON APPLICABLE|Faible|Une application a tenté d’accéder à un numéro bloqué via le service TBS.|  
|4672|576|Faible|Privilèges spéciaux attribués à la nouvelle session.|  
|4673|577|Faible|Un service privilégié a été appelé.|  
|4674|578|Faible|Une opération a été tentée sur un objet privilégié.|  
|4688|592|Faible|Un nouveau processus a été créé.|  
|4689|593|Faible|Un processus s’est arrêté.|  
|4690|594|Faible|Une tentative de dupliquer un handle vers un objet.|  
|4691|595|Faible|Accès indirect à un objet a été demandé.|  
|4694|NON APPLICABLE|Faible|Protection des données protégées pouvant être auditées a été tentée.|  
|4695|NON APPLICABLE|Faible|Unprotection des données protégées pouvant être auditées a été tentée.|  
|4696|600|Faible|Un jeton principal a été attribué à un processus.|  
|4697|601|Faible|Essayez d’installer un service|  
|4698|602|Faible|Création d’une tâche planifiée.|  
|4699|602|Faible|Une tâche planifiée a été supprimée.|  
|4700|602|Faible|Une tâche planifiée a été activée.|  
|4701|602|Faible|Une tâche planifiée a été désactivée.|  
|4702|602|Faible|Une tâche planifiée a été mis à jour.|  
|4704|608|Faible|Un droit utilisateur a été attribué.|  
|4705|609|Faible|Un droit utilisateur a été supprimé.|  
|4707|611|Faible|Une relation d’approbation à un domaine a été supprimée.|  
|4709|NON APPLICABLE|Faible|Les Services IPsec a démarré.|  
|4710|NON APPLICABLE|Faible|Les Services IPsec a été désactivé.|  
|4711|NON APPLICABLE|Faible|Peut contenir l’une des opérations suivantes: moteur PAStore a appliqué la copie mise en cache localement de la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur. Le moteur PAStore a appliqué la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur. Moteur PAStore a appliqué la stratégie IPsec de stockage du Registre local sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer la copie mise en cache localement de stratégie IPsec de stockage ActiveDirectory sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage du Registre local sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Le moteur PAStore n’a pas pu charger la stratégie IPsec sur l’ordinateur de stockage active. Moteur PAStore a chargé le stockage de répertoire stratégie IPsec sur l’ordinateur. Le moteur PAStore n’a pas pu charger la stratégie IPsec sur l’ordinateur de stockage local. Moteur PAStore a chargé le stockage local de stratégie IPsec sur l’ordinateur. Le moteur PAStore recherches de modifications de la stratégie IPsec active et ne détecté aucune modification.|  
|4712|NON APPLICABLE|Faible|IPsec Services a rencontré un problème potentiellement grave.|  
|4717|621|Faible|Accès au système de sécurité a été accordé à un compte.|  
|4718|622|Faible|Un compte d’accès au système de sécurité a été supprimé.|  
|4720|624|Faible|Un compte d’utilisateur a été créé.|  
|4722|626|Faible|Un compte d’utilisateur a été activé.|  
|4723|627|Faible|Une tentative de modifier le mot de passe d’un compte.|  
|4725|629|Faible|Un compte d’utilisateur a été désactivé.|  
|4726|630|Faible|Un compte d’utilisateur a été supprimé.|  
|4728|632|Faible|Un membre a été ajouté à un groupe global de sécurité activée.|  
|4729|633|Faible|Un membre a été supprimé d’un groupe global de sécurité activée.|  
|4730|634|Faible|Un groupe global de sécurité activée a été supprimé.|  
|4731|635|Faible|Un groupe local de sécurité activée a été créé.|  
|4732|636|Faible|Un membre a été ajouté à un groupe local sécurisé.|  
|4733|637|Faible|Un membre a été supprimé d’un groupe local sécurisé.|  
|4734|638|Faible|Un groupe local de sécurité activée a été supprimé.|  
|4738|642|Faible|Un compte d’utilisateur a été modifié.|  
|4740|644|Faible|Un compte d’utilisateur a été verrouillé.|  
|4741|645|Faible|Un compte d’ordinateur a été modifié.|  
|4742|646|Faible|Un compte d’ordinateur a été modifié.|  
|4743|647|Faible|Un compte d’ordinateur a été supprimé.|  
|4744|648|Faible|Un groupe local de sécurité est désactivée a été créé.|  
|4745|649|Faible|Un groupe local de sécurité est désactivée a été modifié.|  
|4746|650|Faible|Un membre a été ajouté à un groupe local de sécurité est désactivée.|  
|4747|651|Faible|Un membre a été supprimé d’un groupe local de sécurité est désactivée.|  
|4748|652|Faible|Un groupe local de sécurité est désactivée a été supprimé.|  
|4749|653|Faible|Un groupe global de sécurité est désactivée a été créé.|  
|4750|654|Faible|Un groupe global de sécurité est désactivée a été modifié.|  
|4751|655|Faible|Un membre a été ajouté à un groupe global de sécurité est désactivée.|  
|4752|656|Faible|Un membre a été supprimé d’un groupe global de sécurité est désactivée.|  
|4753|657|Faible|Un groupe global de sécurité est désactivée a été supprimé.|  
|4756|660|Faible|Un membre a été ajouté à un groupe universel de sécurité activée.|  
|4757|661|Faible|Un membre a été supprimé d’un groupe universel de sécurité activée.|  
|4758|662|Faible|Un groupe universel de sécurité activée a été supprimé.|  
|4759|663|Faible|Un groupe universel de sécurité est désactivée a été créé.|  
|4760|664|Faible|Un groupe universel de sécurité est désactivée a été modifié.|  
|4761|665|Faible|Un membre a été ajouté à un groupe universel de sécurité est désactivée.|  
|4762|666|Faible|Un membre a été supprimé d’un groupe universel de sécurité est désactivée.|  
|4767|671|Faible|Un compte d’utilisateur a été déverrouillé.|  
|4768|672,676|Faible|Un ticket d’authentification Kerberos (TGT) a été demandé.|  
|4769|673|Faible|Un ticket de service Kerberos a été demandé.|  
|4770|674|Faible|Un ticket de service Kerberos a été renouvelé.|  
|4771|675|Faible|Échec de la pré-authentification Kerberos.|  
|4772|672|Faible|Une demande de ticket d’authentification Kerberos a échoué.|  
|4774|678|Faible|Un compte a été mappé pour l’ouverture de session.|  
|4775|679|Faible|Un compte d’ouverture de session n’a pas pu être mappé.|  
|4776|680,681|Faible|Le contrôleur de domaine a tenté de valider les informations d’identification d’un compte.|  
|4777|NON APPLICABLE|Faible|Le contrôleur de domaine n’a pas pu valider les informations d’identification d’un compte.|  
|4778|682|Faible|Une session a été reconnectée à une Station de fenêtre.|  
|4779|683|Faible|Une session déconnectée à partir d’une Station de fenêtre.|  
|4781|685|Faible|Le nom d’un compte a été modifié:|  
|4782|NON APPLICABLE|Faible|Le hachage de mot de passe un compte a accédé.|  
|4783|667|Faible|Un groupe de l’application de base a été créé.|  
|4784|NON APPLICABLE|Faible|Un groupe de l’application de base a été modifié.|  
|4785|689|Faible|Un membre a été ajouté à un groupe de l’application de base.|  
|4786|690|Faible|Un membre a été supprimé d’un groupe de l’application de base.|  
|4787|691|Faible|Un non-membre a été ajouté à un groupe de l’application de base.|  
|4788|692|Faible|Un non-membre a été supprimé d’un groupe de l’application de base.|  
|4789|693|Faible|Un groupe de l’application de base a été supprimé.|  
|4790|694|Faible|Un groupe de requêtes LDAP a été créé.|  
|4793|NON APPLICABLE|Faible|L’API de la vérification de la stratégie de mot de passe a été appelée.|  
|4800|NON APPLICABLE|Faible|La station de travail a été verrouillée.|  
|4801|NON APPLICABLE|Faible|La station de travail a été déverrouillée.|  
|4802|NON APPLICABLE|Faible|L’écran de veille a été appelé.|  
|4803|NON APPLICABLE|Faible|L’écran de veille a été fermée.|  
|4864|NON APPLICABLE|Faible|Une collision de l’espace de noms a été détectée.|  
|4869|773|Faible|Services de certificats ont reçu une requête de certificat soumise à nouveau.|  
|4871|775|Faible|Services de certificats ont reçu une requête pour publier la liste de révocation de certificats (CRL).|  
|4872|776|Faible|Services de certificats publié la liste de révocation de certificats (CRL).|  
|4873|777|Faible|Une extension de demande de certificat est modifié.|  
|4874|778|Faible|Un ou plusieurs attributs de demande de certificat est modifié.|  
|4875|779|Faible|Services de certificats ont reçu une requête d’être arrêté.|  
|4876|780|Faible|Sauvegarde des Services de certificats a démarré.|  
|4877|781|Faible|Sauvegarde des Services de certificat terminée.|  
|4878|782|Faible|Restauration des Services de certificats a démarré.|  
|4879|783|Faible|Terminé la restauration des Services de certificats.|  
|4880|784|Faible|Services de certificats a démarré.|  
|4881|785|Faible|Services de certificats s’est arrêté.|  
|4883|787|Faible|Services de certificats récupéré une clé archivée.|  
|4884|788|Faible|Les Services de certificat importé un certificat dans sa base de données.|  
|4886|790|Faible|Services de certificats ont reçu une requête de certificat.|  
|4887|791|Faible|Services de certificats approuvé une demande de certificat et émis un certificat.|  
|4888|792|Faible|Services de certificats a refusé une demande de certificat.|  
|4889|793|Faible|Services de certificats attribuer le statut d’une demande de certificat en attente.|  
|4891|795|Faible|Une entrée de configuration a changé dans les Services de certificats.|  
|4893|797|Faible|Services de certificats ont archivé une clé.|  
|4894|798|Faible|Services de certificats importé et archivé une clé.|  
|4895|799|Faible|Services de certificats publié le certificat d’autorité de certification aux Services de domaine ActiveDirectory.|  
|4898|802|Faible|Services de certificats ont chargé un modèle.|  
|4902|NON APPLICABLE|Faible|La table de stratégie d’audit par utilisateur a été créée.|  
|4904|NON APPLICABLE|Faible|Une tentative d’inscrire une source d’événement de sécurité.|  
|4905|NON APPLICABLE|Faible|Une tentative a été effectuée pour annuler l’inscription d’une source d’événement de sécurité.|  
|4909|NON APPLICABLE|Faible|Les paramètres de stratégie locale pour la to ont été modifiés.|  
|4910|NON APPLICABLE|Faible|Les paramètres de stratégie de groupe pour le to ont été modifiés.|  
|4928|NON APPLICABLE|Faible|Un contexte de nommage source de réplication ActiveDirectory a été établi.|  
|4929|NON APPLICABLE|Faible|Un contexte de nommage source de réplication ActiveDirectory a été supprimé.|  
|4930|NON APPLICABLE|Faible|Un contexte de nommage source de réplication ActiveDirectory a été modifié.|  
|4931|NON APPLICABLE|Faible|Un contexte de nommage destination de réplication ActiveDirectory a été modifié.|  
|4932|NON APPLICABLE|Faible|Synchronisation d’un réplica d’un contexte de nommage ActiveDirectory a commencé.|  
|4933|NON APPLICABLE|Faible|Synchronisation d’un réplica d’un contexte de nommage ActiveDirectory est terminée.|  
|4934|NON APPLICABLE|Faible|Les attributs d’un objet ActiveDirectory ont été répliqués.|  
|4935|NON APPLICABLE|Faible|Échec de la réplication commence.|  
|4936|NON APPLICABLE|Faible|Échec de la réplication se termine.|  
|4937|NON APPLICABLE|Faible|Un objet en attente a été supprimé d’un réplica.|  
|4944|NON APPLICABLE|Faible|La stratégie suivante était active lorsque le pare-feu Windows est démarré.|  
|4945|NON APPLICABLE|Faible|Une règle a été répertoriée lorsque le pare-feu Windows est démarré.|  
|4946|NON APPLICABLE|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été ajoutée.|  
|4947|NON APPLICABLE|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été modifiée.|  
|4948|NON APPLICABLE|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été supprimée.|  
|4949|NON APPLICABLE|Faible|Paramètres du pare-feu Windows ont été restaurés sur les valeurs par défaut.|  
|4950|NON APPLICABLE|Faible|Un paramètre du pare-feu Windows a changé.|  
|4951|NON APPLICABLE|Faible|Une règle a été ignorée car son numéro de version principale n’a pas été reconnue par le pare-feu Windows.|  
|4952|NON APPLICABLE|Faible|Parties d’une règle ont été ignorés, car son numéro de version mineure n’a pas été reconnue par le pare-feu Windows. Les autres parties de la règle seront appliquées.|  
|4953|NON APPLICABLE|Faible|Une règle a été ignorée par le pare-feu Windows, car il a pas pu analyser la règle.|  
|4954|NON APPLICABLE|Faible|Paramètres de stratégie de groupe du pare-feu Windows ont été modifiés. Les nouveaux paramètres ont été appliqués.|  
|4956|NON APPLICABLE|Faible|Le pare-feu Windows a changé le profil actif.|  
|4957|NON APPLICABLE|Faible|Le pare-feu Windows n’a pas appliqué la règle suivante:|  
|4958|NON APPLICABLE|Faible|Le pare-feu Windows n’a pas appliqué la règle suivante, car la règle fait référence à des éléments non configurés sur cet ordinateur:|  
|4979|NON APPLICABLE|Faible|Associations de sécurité de Mode principal IPsec et en Mode étendu a été établies.|  
|4980|NON APPLICABLE|Faible|Associations de sécurité de Mode principal IPsec et en Mode étendu a été établies.|  
|4981|NON APPLICABLE|Faible|Associations de sécurité de Mode principal IPsec et en Mode étendu a été établies.|  
|4982|NON APPLICABLE|Faible|Associations de sécurité de Mode principal IPsec et en Mode étendu a été établies.|  
|4985|NON APPLICABLE|Faible|L’état d’une transaction a changé.|  
|5024|NON APPLICABLE|Faible|Le Service pare-feu Windows a démarré avec succès.|  
|5025|NON APPLICABLE|Faible|Le Service pare-feu Windows a été arrêté.|  
|5031|NON APPLICABLE|Faible|Le Service pare-feu Windows bloqué une application d’accepter les connexions entrantes sur le réseau.|  
|5032|NON APPLICABLE|Faible|Le pare-feu Windows n’a pas pu notifier l’utilisateur qu’il a bloqué une application d’accepter les connexions entrantes sur le réseau.|  
|5033|NON APPLICABLE|Faible|Le pilote du pare-feu Windows a démarré avec succès.|  
|5034|NON APPLICABLE|Faible|Le pilote du pare-feu Windows a été arrêté.|  
|5039|NON APPLICABLE|Faible|Une clé de Registre a été virtualisée.|  
|5040|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble de l’authentification a été ajouté.|  
|5041|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble de l’authentification a été modifié.|  
|5042|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble de l’authentification a été supprimé.|  
|5043|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été ajoutée.|  
|5044|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été modifiée.|  
|5045|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été supprimée.|  
|5046|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été ajouté.|  
|5047|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été modifié.|  
|5048|NON APPLICABLE|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été supprimé.|  
|5050|NON APPLICABLE|Faible|Une tentative de désactivation par programme le pare-feu Windows à l’aide d’un appel à InetFwProfile.FirewallEnabled(False)|  
|5051|NON APPLICABLE|Faible|Un fichier a été virtualisé.|  
|5056|NON APPLICABLE|Faible|Un auto-test chiffrement a été effectué.|  
|5057|NON APPLICABLE|Faible|Échec d’une opération de primitive cryptographique.|  
|5058|NON APPLICABLE|Faible|Opération de fichier de clé.|  
|5059|NON APPLICABLE|Faible|Opération de migration des clés.|  
|5060|NON APPLICABLE|Faible|Échec de l’opération de vérification.|  
|5061|NON APPLICABLE|Faible|Opération de chiffrement.|  
|5062|NON APPLICABLE|Faible|Un en mode noyau chiffrement qu'autotest a été effectué.|  
|5063|NON APPLICABLE|Faible|Une opération de fournisseur de chiffrement a été tentée.|  
|5064|NON APPLICABLE|Faible|Une opération de contexte de chiffrement a été tentée.|  
|5065|NON APPLICABLE|Faible|Une modification de contexte de chiffrement a été tentée.|  
|5066|NON APPLICABLE|Faible|Une opération de fonction de chiffrement a été tentée.|  
|5067|NON APPLICABLE|Faible|Une modification de la fonction de chiffrement a été tentée.|  
|5068|NON APPLICABLE|Faible|Une opération de fournisseur de fonction de chiffrement a été tentée.|  
|5069|NON APPLICABLE|Faible|Une opération de propriété de fonction de chiffrement a été tentée.|  
|5070|NON APPLICABLE|Faible|Une modification de propriété de fonction de chiffrement a été tentée.|  
|5125|NON APPLICABLE|Faible|Une demande a été soumise au Service répondeurs OCSP|  
|5126|NON APPLICABLE|Faible|Certificat de signature a été mis à jour automatiquement par le Service de répondeurs OCSP|  
|5127|NON APPLICABLE|Faible|Le fournisseur de révocation OCSP correctement les mises à jour les informations de révocation|  
|5136|566|Faible|Un objet service d’annuaire a été modifié.|  
|5137|566|Faible|Un objet service d’annuaire a été créé.|  
|5138|NON APPLICABLE|Faible|Un objet service d’annuaire a été récupéré.|  
|5139|NON APPLICABLE|Faible|Un objet service d’annuaire a été déplacé.|  
|5140|NON APPLICABLE|Faible|Un objet de partage réseau a accédé.|  
|5141|NON APPLICABLE|Faible|Un objet service d’annuaire a été supprimé.|  
|5152|NON APPLICABLE|Faible|La plateforme de filtrage Windows bloqué un paquet.|  
|5153|NON APPLICABLE|Faible|Un filtre plus restrictif de la plateforme de filtrage Windows a bloqué un paquet.|  
|5154|NON APPLICABLE|Faible|La plateforme de filtrage Windows a autorisé une application ou un service à l’écoute sur un port pour les connexions entrantes.|  
|5155|NON APPLICABLE|Faible|La plateforme de filtrage Windows a bloqué une application ou un service d’écoute sur un port pour les connexions entrantes.|  
|5156|NON APPLICABLE|Faible|La plateforme de filtrage Windows a autorisé une connexion.|  
|5157|NON APPLICABLE|Faible|La plateforme de filtrage Windows a bloqué une connexion.|  
|5158|NON APPLICABLE|Faible|La plateforme de filtrage Windows a autorisé une liaison à un port local.|  
|5159|NON APPLICABLE|Faible|La plateforme de filtrage Windows a bloqué une liaison à un port local.|  
|5378|NON APPLICABLE|Faible|La délégation d’informations d’identification demandées a été interdite par la stratégie.|  
|5440|NON APPLICABLE|Faible|L’appel suivant a été présente lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5441|NON APPLICABLE|Faible|Le filtre suivant a été présent lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5442|NON APPLICABLE|Faible|Le fournisseur suivant a été présent lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5443|NON APPLICABLE|Faible|Le contexte du fournisseur suivant a été présent lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5444|NON APPLICABLE|Faible|La sous-couche suivante a été présente lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5446|NON APPLICABLE|Faible|Une légende de la plateforme de filtrage Windows a été modifiée.|  
|5447|NON APPLICABLE|Faible|Un filtre de la plateforme de filtrage Windows a été modifié.|  
|5448|NON APPLICABLE|Faible|Un fournisseur de la plateforme de filtrage Windows a été modifié.|  
|5449|NON APPLICABLE|Faible|Un contexte de fournisseur de plateforme de filtrage Windows a été modifié.|  
|5450|NON APPLICABLE|Faible|Une sous-couche plateforme de filtrage Windows a été modifiée.|  
|5451|NON APPLICABLE|Faible|Une association de sécurité en Mode rapide IPsec a été établie.|  
|5452|NON APPLICABLE|Faible|Une association de sécurité en Mode rapide IPsec a pris fin.|  
|5456|NON APPLICABLE|Faible|Le moteur PAStore a appliqué la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur.|  
|5457|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur.|  
|5458|NON APPLICABLE|Faible|Le moteur PAStore appliqué la copie mise en cache localement de la stratégie IPsec de stockage ActiveDirectory sur l’ordinateur.|  
|5459|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu appliquer la copie mise en cache localement de stratégie IPsec de stockage ActiveDirectory sur l’ordinateur.|  
|5460|NON APPLICABLE|Faible|Moteur PAStore a appliqué la stratégie IPsec de stockage du Registre local sur l’ordinateur.|  
|5461|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage du Registre local sur l’ordinateur.|  
|5462|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5463|NON APPLICABLE|Faible|Le moteur PAStore recherches de modifications de la stratégie IPsec active et ne détecté aucune modification.|  
|5464|NON APPLICABLE|Faible|Le moteur PAStore recherches de modifications de la stratégie IPsec active, a détecté des modifications et appliqué les Services IPsec.|  
|5465|NON APPLICABLE|Faible|Le moteur PAStore a reçu un contrôle pour recharger forcée de la stratégie IPsec et réussi le contrôle.|  
|5466|NON APPLICABLE|Faible|Le moteur PAStore recherches de modifications de la stratégie IPsec ActiveDirectory, déterminé qu’ActiveDirectory ne peut pas être atteint et utilisera la copie mise en cache de la stratégie IPsec ActiveDirectory. Toutes les modifications apportées à la stratégie IPsec ActiveDirectory depuis la dernière interrogation n’a pas pu être appliquée.|  
|5467|NON APPLICABLE|Faible|Le moteur PAStore recherches de modifications de la stratégie IPsec ActiveDirectory, déterminé qu’ActiveDirectory peut être atteint et ne trouvé aucune modification de la stratégie. La copie mise en cache de la stratégie IPsec ActiveDirectory est plus utilisée.|  
|5468|NON APPLICABLE|Faible|Le moteur PAStore recherches de modifications de la stratégie IPsec ActiveDirectory, déterminé qu’ActiveDirectory peut être atteint et trouver les modifications apportées à la stratégie et applique les modifications. La copie mise en cache de la stratégie IPsec ActiveDirectory est plus utilisée.|  
|5471|NON APPLICABLE|Faible|Moteur PAStore a chargé le stockage local de stratégie IPsec sur l’ordinateur.|  
|5472|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu charger la stratégie IPsec sur l’ordinateur de stockage local.|  
|5473|NON APPLICABLE|Faible|Moteur PAStore a chargé le stockage de répertoire stratégie IPsec sur l’ordinateur.|  
|5474|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu charger la stratégie IPsec sur l’ordinateur de stockage active.|  
|5477|NON APPLICABLE|Faible|Le moteur PAStore n’a pas pu ajouter le filtre de mode rapide.|  
|5479|NON APPLICABLE|Faible|Les Services IPsec a été arrêté avec succès. L’arrêt des Services IPsec peut mettre l’ordinateur à un plus grand risque d’attaque de réseau ou exposer l’ordinateur à des risques de sécurité.|  
|5632|NON APPLICABLE|Faible|Une demande a été effectuée pour s’authentifier auprès d’un réseau sans fil.|  
|5633|NON APPLICABLE|Faible|Une demande a été effectuée pour s’authentifier auprès d’un réseau câblé.|  
|5712|NON APPLICABLE|Faible|Un appel de procédure distante (RPC) a été tentée.|  
|5888|NON APPLICABLE|Faible|Un objet dans le catalogue COM + a été modifié.|  
|5889|NON APPLICABLE|Faible|Un objet a été supprimé depuis le catalogue COM +.|  
|5890|NON APPLICABLE|Faible|Un objet a été ajouté dans le catalogue COM +.|  
|6008|NON APPLICABLE|Faible|L’arrêt précédent du système inattendu.|  
|6144|NON APPLICABLE|Faible|Stratégie de sécurité dans les objets de stratégie de groupe a été appliquée avec succès.|  
|6272|NON APPLICABLE|Faible|Serveur de stratégie réseau autorisés à accéder à un utilisateur.|  
|NON APPLICABLE|561|Faible|Un handle vers un objet a été demandé.|  
|NON APPLICABLE|563|Faible|Ouvrir pour la suppression d’objet|  
|NON APPLICABLE|625|Faible|Type de compte d’utilisateur modifié|  
|NON APPLICABLE|613|Faible|Démarré l’agent de stratégie IPsec|  
|NON APPLICABLE|614|Faible|Agent de stratégie IPsec désactivé|  
|NON APPLICABLE|615|Faible|Agent de stratégie IPsec|  
|NON APPLICABLE|616|Faible|Agent de stratégie IPsec a rencontré une défaillance grave|  
|24577|NON APPLICABLE|Faible|Chiffrement du volume de démarrage|  
|24578|NON APPLICABLE|Faible|Chiffrement du volume arrêté|  
|24579|NON APPLICABLE|Faible|Chiffrement du volume terminée|  
|24580|NON APPLICABLE|Faible|Déchiffrement du volume de démarrage|  
|24581|NON APPLICABLE|Faible|Déchiffrement du volume arrêté|  
|24582|NON APPLICABLE|Faible|Déchiffrement du volume terminée|  
|24583|NON APPLICABLE|Faible|Thread de travail de conversion pour le volume de démarrage|  
|24584|NON APPLICABLE|Faible|Thread de travail de conversion pour volume temporairement interrompu|  
|24588|NON APPLICABLE|Faible|L’opération de conversion sur le volume %2 a rencontré une erreur de secteur défectueux. Vérifiez les données stockées sur ce volume|  
|24595|NON APPLICABLE|Faible|Volume %2 contient des clusters défectueux. Ces clusters seront ignorées lors de la conversion.|  
|24621|NON APPLICABLE|Faible|À la vérification de l’état initiale: restauration de transaction de conversion de volume sur %2.|  
|5049|NON APPLICABLE|Faible|Une Association de sécurité IPsec a été supprimée.|  
|5478|NON APPLICABLE|Faible|Les Services IPsec a démarré avec succès.|  
  
> [!NOTE]  
> Reportez-vous à [article du Support Microsoft947226](https://support.microsoft.com/kb/947226) pour les listes de nombreux ID d’événement de sécurité et leur signification.  
>   
> Exécutez **wevtutil gp Microsoft-Windows-Security-Auditing /ge /gm: true** pour obtenir une liste très détaillée de la sécurité de tous les ID d’événement  
  
Pour plus d’informations sur les ID d’événement de sécurité Windows et leur signification, voir les articles du Support Microsoft [Description des événements de sécurité dans WindowsVista et Windows Server2008](https://support.microsoft.com/kb/947226) et [Description des événements de sécurité dans Windows7 et Windows Server2008R2](https://support.microsoft.com/kb/977519). Vous pouvez également télécharger [événements d’Audit de sécurité pour Windows7 et Windows Server2008R2](https://www.microsoft.com/download/details.aspx?id=21561) et [Windows8 et Windows Server2012 sécurité détails de l’événement](https://www.microsoft.com/download/details.aspx?id=35753), qui fournissent les événements des informations détaillées sur les systèmes d’exploitation référencés dans le format de la feuille de calcul.  
  


