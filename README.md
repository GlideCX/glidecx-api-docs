# 👋 Welcome to GlideCX's REST API

REST API allows you to access your GlideCX data. Request methods are used in accordance with HTTP:

* GET is used to read one or more entities;
* POST is used to create or update entities;
* DELETE is used to delete entities;
* Responses use standard HTTP codes.

Before you start using the API, read through the following five basics 👇 

## 1️⃣ Base API Domain

GlideCX's API is provided on the following HTTPS-only domain:

```
https://api.glidecx.com/user/api/v1/
```

## 2️⃣ Authentication

You should authenticate all your HTTP requests to GlideCX's API with the help of your API key.

To get your API key, go to Account → API Keys and copy your API Key from there.

[Screenshot coming soon]

```
Authorization: Bearer YOUR_API_KEY
```

You don't need to do any extra manipulations with your API key. It is permanent and never expires until reset from the dashboard.

Make sure you pass the entire key without any spaces or extra symbols.

## 3️⃣ Rate limiting

In order to protect Cloutly from API traffic spikes that could put our database at risk we utilise rate-limiting. When you get rate-limited, you will start receiving ***429 Too Many Requests*** HTTP errors in response to your requests.

The default overall requests limit is ***200 per minute***

## 4️⃣ Errors

Depending on the exact method, you can get one of the following error status codes and responses: 

### 400 Bad Request

The request could not be understood by GlideCX's server due to malformed syntax or invalid attribute value. DO NOT repeat such requests without modifications.

### 401 Unauthorized

This means that something is wrong with your API key or it's not added to the request headers at all. Make sure you pass it as described in the ***Authentication*** section above.

### 404 Not Found

We were unable to find anything matching your request.

### 429 Too Many Requests

You reached a rate-limit of 100 API calls per minute.

## 5️⃣ Endpoints

Here's the list of all available endpoints you can use to access and control your GlideCX data.

## ✅ GET /businesses

Get list of businesses (locations) and sources connected to this Cloutly account.

### Request

URL

```
GET https://api.glidecx.com/user/api/v1/businesses
```

Headers

```
Authorization: Bearer YOUR_API_KEY
```

Query Parameters

N/A

### Response

```
[
  {
    "id": "41f92685-9f7d-4e81-9a9f-0f9613db2c8e",
    "location_name": "My Business Name",
    "location_slug": "fazal-dins-pharma-plus",
    "display_location_name": "US - My Business Name",
    "logo_url": "LOCATION_LOGO_IMAGE",
    "cover_url": "LOCATION_COVER_IMAGE",
    "sources": [
        "google",
        "facebook",
        "yelp"
    ],
    ...
  }
]
```

## ✅ GET /reviews

Pull a list of reviews for all your connected sources in GlideCX. Note: you need to connect the source as an integration in the dashboard first.

### Request

URL

```
GET https://api.glidecx.com/user/api/v1/reviews
```

Headers

```
Authorization: Bearer YOUR_API_KEY
```

Query Parameters:

* ***source*** google | facebook - omit field for all sources. The source can be found from the sources property when calling GET /businesses
* ***show_on_widget*** - true | false - pull reviews that are marked as show on widget only
* ***location** add the location id to filter the reviews from specific location/business. The location id can be found from the id property when calling GET /businesses
* ***sort*** - desc | asc - ordering of reviews

### Response

```
{
    "count": 21,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": "d18e20e5-9db7-47e8-906b-2e9bf88b81e7",
            "source": "google",
            "show_on_widget": true,
            "author_name": "David Alexander",
            "profile_photo_url": "review_author_image",
            "rating": 5.0,
            "review_text": "here review text will be displayed",
            "is_responded": false,
            "published_at": "2020-08-15T15:55:12.771374Z",
            "created_at": "2024-08-15T15:55:12.771612Z",
            "updated_at": "2024-08-15T15:55:12.771616Z",
            ...
        },
    ]
}
```

Notes: 

* ***reviews*** is ordered by published_at: desc, which means the latest review will always be at the beginning of the list.
* Most fields a nullable so remember to unwrap fields if you are expecting a required value
* The `...` indicates that new fields may be added over time.
* We will consider changing field names or removing fields to be a breaking change and will publish a new version (V2 in this case).


