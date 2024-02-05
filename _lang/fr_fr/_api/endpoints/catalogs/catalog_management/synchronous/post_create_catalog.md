---
nav_title: "POST: Créer un catalogue"
article_title: "POST: Créer un catalogue"
search_tag: Endpoint
page_order: 3

layout: api_page
page_type: reference
description: "Cet article décrit les détails du point de terminaison Créer un catalogue Braze."

---
{% api %}
# Créer un catalogue
{% apimethod post %}
/catalogs
{% endapimethod %}

> Utilisez ce paramètre pour créer un catalogue.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#af9f3e2d-b7e7-49e7-aa64-f4652892be6e {% endapiref %}

## Prérequis

Pour utiliser ce point de terminaison, vous aurez besoin d'une [clé API]({{site.baseurl}}/api/basics#rest-api-key/) avec l'autorisation `catalogs.create`.

## Limite tarifaire

{% multi_lang_include rate_limits.md endpoint='synchronous catalog' %}

## Paramètres de requête

| Paramètre | Obligatoire | Type de données | Description |
|---|---|---|---|
| `catalogs` | Obligatoire | Array | Un tableau qui contient des objets de catalogue. Un seul objet catalogue est autorisé pour cette requête. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

### Paramètres des objets du catalogue

| Paramètre | Obligatoire | Type de données | Description |
|---|---|---|---|
| `name` | Obligatoire | String | Le nom du catalogue que vous souhaitez créer. |
| `description` | Obligatoire | String | La description du catalogue que vous souhaitez créer. |
| `fields` | Obligatoire | Array | Un tableau d'objets où l'objet contient des clés `name` et `type`. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Exemple de demande
```
curl --location --request POST 'https://rest.iad-03.braze.com/catalogs' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY' \
--data-raw '{
  "catalogs": [
    {
      "name": "restaurants",
      "description": "My Restaurants",
      "fields": [
        {
          "name": "id",
          "type": "string"
        },
        {
          "name": "Name",
          "type": "string"
        },
        {
          "name": "City",
          "type": "string"
        },
        {
          "name": "Cuisine",
          "type": "string"
        },
        {
          "name": "Rating",
          "type": "number"
        },
        {
          "name": "Loyalty_Program",
          "type": "boolean"
        },
        {
          "name": "Location",
          "type": "object"
        },
        {
          "name": "Created_At",
          "type": "time"
        }
      ]
    }
  ]
}'
```

## Réponse

Il existe deux réponses de code d'état pour ce paramètre : `201` et `400`.

### Exemple de réponse au succès

Le `201` de code d'état pourrait renvoyer le corps de réponse suivant.

```json
{
  "catalogs": [
    {
      "description": "My Restaurants",
      "fields": [
        {
          "name": "id",
          "type": "string"
        },
        {
          "name": "Name",
          "type": "string"
        },
        {
          "name": "City",
          "type": "string"
        },
        {
          "name": "Cuisine",
          "type": "string"
        },
        {
          "name": "Rating",
          "type": "number"
        },
        {
          "name": "Loyalty_Program",
          "type": "boolean"
        },
        {
          "name": "Location",
          "type": "object"
        },
        {
          "name": "Created_At",
          "type": "time"
        }
      ],
      "name": "restaurants",
      "num_items": 0,
      "updated_at": "2022-11-02T20:04:06.879+00:00"
    }
  ],
  "message": "success"
}
```

### Exemple de réponse d'erreur

Le `400` de code d'état pourrait renvoyer le corps de réponse suivant. Reportez-vous à la section [Dépannage](#troubleshooting) pour plus d'informations sur les erreurs que vous pouvez rencontrer.

```json
{
  "errors": [
    {
      "id": "catalog-name-already-exists",
      "message": "A catalog with that name already exists",
      "parameters": [
        "name"
      ],
      "parameter_values": [
        "restaurants"
      ]
    }
  ],
  "message": "Invalid Request"
}
```

## Dépannage

Le tableau suivant répertorie les erreurs retournées possibles et les étapes de dépannage associées.

| Erreur | Dépannage |
| --- | --- |
| `catalog-array-invalid` | `catalogs` doit être un tableau d'objets. |
| `catalog-name-already-exists` | Le catalogue avec ce nom existe déjà. |
| `catalog-name-too-large`  | La limite de caractères pour un nom de catalogue est de 250. |
| `description-too-long` | La limite de caractères pour la description est de 250. |
| `field-names-not-unique` | Le même nom de champ est référencé deux fois. |
| `field-names-too-large` | La limite de caractères pour un nom de champ est de 250. |
| `id-not-first-column` | Le `id` doit être le premier champ du tableau. Vérifiez que le type est une chaîne de caractères. |
| `invalid-catalog-name` | Le nom du catalogue ne peut inclure que des lettres, des chiffres, des traits d'union et des soulignements. |
| `invalid-field-names` | Les champs ne peuvent inclure que des lettres, des chiffres, des traits d'union et des soulignements. |
| `invalid-field-types` | Assurez-vous que les types de champs sont valides. |
| `invalid-fields` | `fields` n'est pas formaté correctement. |
| `too-many-catalog-atoms` | Vous ne pouvez créer qu'un catalogue par demande. |
| `too-many-fields` | La limite du nombre de champs est de 500. |
{: .reset-td-br-1 .reset-td-br-2}

{% endapi %}
