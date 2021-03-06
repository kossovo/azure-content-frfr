<properties 
    pageTitle="Files d’attente de lettres mortes Service Bus | Microsoft Azure" 
    description="Vue d’ensemble des files d’attente de lettres mortes Azure Service Bus" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="06/21/2016"
    ms.author="clemensv;sethm"/>

# Vue d’ensemble des files d’attente de lettres mortes Service Bus

Les files d’attente Service Bus et les abonnements aux rubriques fournissent une sous-file d’attente secondaire, appelée *file d’attente de lettres mortes*. La file d’attente de lettres mortes n’a pas besoin d’être explicitement créée et ne peut pas être supprimée ou gérée indépendamment de l’entité principale.

L’objectif de la file d’attente de lettres mortes est de conserver les messages qui ne peuvent pas être remis aux destinataires ou simplement les messages qui n’ont pas pu être traités. Les messages peuvent alors être supprimés de la file d’attente de lettres mortes et inspectés. Une application peut, à l’aide d’un opérateur, corriger les problèmes et renvoyer le message, consigner le fait qu’une erreur s’est produite et/ou prendre des mesures correctives.

Du point de vue de l’API et du protocole, la file d’attente de lettres mortes est essentiellement similaire à une autre file d’attente, sauf que les messages peuvent être envoyés uniquement via le mouvement de lettres mortes de l’entité parente. En outre, la durée de vie n’est pas observée, et vous ne pouvez pas supprimer un message d’une file d’attente de lettres mortes. La file d’attente de lettres mortes prend entièrement en charge la remise de verrou d’affichage et les opérations transactionnelles.

Notez qu’il n’y a aucun nettoyage automatique de la file d’attente de lettres mortes. Les messages restent dans la file d’attente de lettres mortes jusqu’à ce que vous les récupériez explicitement et que vous appeliez [Complete()](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.completeasync.aspx) sur le message de lettres mortes.

## Déplacer des messages vers la file d’attente de lettres mortes

Plusieurs activités dans Service Bus entraînent l’envoi des messages dans la file d’attente de lettres mortes à partir du moteur de messagerie lui-même. Une application peut également explicitement envoyer des messages dans la file d’attente de lettres mortes.

Comme le message est déplacé par le service broker, deux propriétés sont ajoutées au message quand le service broker appelle sa version interne de la méthode [DeadLetter](https://msdn.microsoft.com/library/azure/hh291941.aspx) sur le message : `DeadLetterReason` et `DeadLetterErrorDescription`.

Les applications peuvent définir leurs propres codes pour la propriété `DeadLetterReason`, mais le système définit les valeurs suivantes.

| Condition | DeadLetterReason | DeadLetterErrorDescription |
|---------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|----------------------------------------------------------------------------------|
| Toujours | HeaderSizeExceeded | Le quota de taille pour ce flux a été dépassé. |
| !TopicDescription.<br />EnableFilteringMessagesBeforePublishing et SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions | exception.GetType().Name | exception.Message |
| EnableDeadLetteringOnMessageExpiration | TTLExpiredException | Le message a expiré et a été placé dans la file d’attente de lettres mortes. |
| SubscriptionDescription.RequiresSession | L’ID de session a la valeur null. | L’entité activée dans la session n’autorise pas les messages dont l’identificateur de session a la valeur null. |
| !dead letter queue | MaxTransferHopCountExceeded | Null |
| Mise en file d’attente de lettres mortes explicite par l’application | Spécifié par l’application | Spécifié par l’application |

## Dépassement de MaxDeliveryCount

Les files d’attente et les abonnements ont chacun une propriété [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) et [SubscriptionDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount.aspx), respectivement. La valeur par défaut est 10. Chaque fois qu’un message a été remis sous un verrou ([ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)), mais a été explicitement abandonné ou le verrou a expiré, la propriété [BrokeredMessage.DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx) du message est incrémentée. Quand [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx) dépasse [MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx), le message est déplacé vers la file d’attente de lettres mortes avec le code motif `MaxDeliveryCountExceeded`.

Ce comportement ne peut pas être désactivé, mais vous pouvez définir [MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) sur un très grand nombre.

## Dépassement de TimeToLive

Quand la propriété [QueueDescription.EnableDeadLetteringOnMessageExpiration](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration.aspx) ou [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.enabledeadletteringonmessageexpiration.aspx) est définie sur **true** (la valeur par défaut est **false**), tous les messages arrivant à expiration sont déplacés vers la file d’attente de lettres mortes avec le code motif `TTLExpiredException`.

Notez que les messages ayant expiré sont uniquement purgés et donc transférés dans la file d’attente de lettres mortes quand au moins un destinataire actif effectue une collecte dans la file d’attente principale ou l’abonnement. Ce comportement est normal.

## Erreurs pendant le traitement des règles d’abonnement

Quand la propriété [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.enabledeadletteringonfilterevaluationexceptions.aspx) est activée pour un abonnement, les erreurs qui surviennent pendant l’exécution des règles de filtre SQL d’un abonnement sont capturées dans la file d’attente de lettres mortes avec le message incriminé.

## Mise en file d’attente de lettres mortes au niveau de l’application

En plus des fonctionnalités de file d’attente de lettres mortes fournies par le système, les applications peuvent utiliser la file d’attente de lettres mortes pour refuser explicitement les messages inacceptables. Il peut s’agir de messages qui ne peuvent pas être traités correctement en raison d’un problème système, de messages qui contiennent des charges utiles incorrectement formées ou de messages qui n’ont pas pu être authentifiés lors de l’utilisation d’un schéma de sécurité au niveau du message.

## Exemple

L’extrait de code suivant crée un destinataire de message. Dans la boucle de réception de la file d’attente principale, le code récupère le message avec [Receive(TimeSpan.Zero)](https://msdn.microsoft.com/library/azure/dn130350.aspx), qui demande au service broker de retourner immédiatement les messages disponibles ou de ne retourner aucun résultat. Si le code reçoit un message, il l’abandonne immédiatement, ce qui incrémente le `DeliveryCount`. Une fois que le système déplace le message vers la file d’attente de lettres mortes, la file d’attente principale est vide et la boucle se ferme, car [ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130350.aspx) retourne **null**.

```
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## Étapes suivantes

Pour plus d’informations sur les files d’attente de lettres mortes Service Bus, consultez les articles suivants :

- [Prise en main des files d’attente Service Bus](service-bus-dotnet-get-started-with-queues.md)
- [Comparaison des files d’attente Azure et Service Bus](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

<!---HONumber=AcomDC_0622_2016-->