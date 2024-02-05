---
nav_title: "GET : Lister l'état de la synchronisation des travaux"
article_title: "GET : Lister l'état de la synchronisation des travaux"
search_tag: Endpoint
page_order: 1
alias: /api/cdi/get_job_sync/
layout: api_page
page_type: reference
description: "Cet article présente les détails du point de terminaison Braze de synchronisation de l'état des tâches de la liste."

---
{% api %}
# Liste l'état de la synchronisation des tâches
{% apimethod get %}
/cdi/integrations/{integration_id}/job_sync_status
{% endapimethod %}

> Utilise ce point de terminaison pour renvoyer une liste des états de synchronisation passés pour une intégration donnée.

{% alert note %}
Pour utiliser ce point de terminaison, tu dois générer une clé API avec l'autorisation `cdi.integration_job_status`.
{% endalert %}

## Limite du taux

{% multi_lang_include rate_limits.md endpoint='cdi job sync status' %}

## Paramètres du chemin

| Paramètre | Exigée | Type de données | Description |
|---|---|---|---|
| `integration_id` | Exigée | Chaîne | ID d'intégration. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Paramètres de la requête

Chaque appel à ce point d'extrémité renverra 10 éléments. Pour une intégration avec plus de 10 synchronisations, utilise l'en-tête `Link` pour récupérer les données sur la page suivante, comme le montre l'exemple de réponse suivant.

| Paramètre | Exigée | Type de données | Description |
|---|---|---|---|
| `cursor` | En option | Chaîne | Détermine la pagination de l'état de synchronisation. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Exemple de demande

### Sans curseur

```
curl --location --request GET 'https://rest.iad-03.braze.com/cdi/integrations/00000000-0000-0000-0000-000000000000/job_sync_status' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY'
```

### Avec le curseur

```
curl --location --request GET 'https://rest.iad-03.braze.com/cdi/integrations/00000000-0000-0000-0000-000000000000/job_sync_status?cursor=c2tpcDow' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY'
```

## Réponse

### Exemple de réponse positive

Le code de statut `200` pourrait renvoyer le corps de réponse suivant.

{% alert note %}
L'en-tête `Link` n'existe pas si le nombre total de synchronisations est inférieur ou égal à 10. Pour les appels sans curseur, `prev` ne s'affiche pas. Lorsque tu regardes la dernière page d'articles, `next` ne s'affiche pas.
{% endalert %}

```
Link: </cdi/integrations/00000000-0000-0000-0000-000000000000/job_sync_status?cursor=c2tpcDow>; rel="prev",</cdi/integrations00000000-0000-0000-0000-000000000000/job_sync_status?cursor=c2tpcDoxMDA=>; rel="next"
```

```json
{
  "results": [
    {
        "job_status": (string) status of the sync, see below for explanation of different statuses,
        "sync_start_time": (string) time the sync started in ISO 8601,
        "sync_finish_time": (string) time the sync finished in ISO 8601,
        "last_timestamp_synced": (string) last UPDATED_AT timestamp processed by the sync in ISO 8601,
        "rows_synced": (integer) number of rows successfully synced to Braze,
        "rows_failed_with_errors": (integer) number of rows failed because of errors,
    },
  ],
  "message": "success"
}
```

| état_du_travail | Explication |
| --- | --- |
| `running` | Le travail est en cours d'exécution. |
| `success` | Toutes les lignes ont été synchronisées avec succès. |
| `partial` | Certaines lignes n'ont pas pu être synchronisées en raison d'erreurs. |
| `error` | Aucune ligne n'a été synchronisée. |
| `config_error` | Il y a eu une erreur dans la configuration de l'intégration. Vérifie ta configuration d'intégration. |
{: .reset-td-br-1 .reset-td-br-2}

## Dépannage

Le tableau suivant répertorie les erreurs renvoyées possibles et les étapes de dépannage associées.

| Erreur | Dépannage |
| --- | --- |
| `400 Invalid cursor` | Vérifie que ton site `cursor` est valide. |
| `400 Invalid integration ID` | Vérifie que ton site `integration_id` est valide. |
{: .reset-td-br-1 .reset-td-br-2}

Pour les codes d'état supplémentaires et les messages d'erreur associés, tu peux te référer à la rubrique [Erreurs fatales et réponses]({{site.baseurl}}/api/errors/#fatal-errors).

{% endapi %}
