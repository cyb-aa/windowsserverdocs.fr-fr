> [!Note] 
> Lors de l'exécution de l'outil de diagnostic Guarded Fabric (Get-HgsTrace -RunDiagnostics), un statut incorrect peut être renvoyé, prétendant que la configuration HTTPS est interrompue alors qu’elle n'est en fait ni interrompue, ni utilisée. Cette erreur peut être retournée quel que soit le mode d’attestation « SGH ». Les causes principales possibles sont les suivantes :
>
> - HTTPS est en effet configuré de manière incorrecte/interrompu<br>
> - Vous utilisez une attestation approuvée par l’administrateur et la relation d’approbation est rompue<br>
> &nbsp;&nbsp;&nbsp;&nbsp;-Il s’agit de la configuration correcte, incorrecte ou inutilisée du protocole HTTPs.<br>
>
> Notez que les diagnostics ne renvoient ce statut incorrect que lorsqu'ils visent un hôte Hyper-V. Si les diagnostics ciblent le Service Guardian hôte, le statut renvoyé sera correct.

<!-- Appears in guarded-fabric-setting-up-the-host-guardian-service-hgs.md and guarded-fabric-troubleshoot-diagnostics.md
-->
