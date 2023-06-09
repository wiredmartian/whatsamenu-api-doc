
# Table of Contents
<!-- TOC -->

- [WhatsAMenu API](#whatsamenu-api)
    - [Supported Enums](#supported-enums)
      - [Used Status Codes](#http-status-codes) 
      - [Upload Types](#upload-type-entities)
      - [Provinces](#provinces)
    - [Authentication](#authentication)
      - [Accepted Headers](#accepted-headers)
    - [API](#api)
      - [Users](#users)
        - [Create New User Account](#new-user-account)
        - [User Login](#login-user)                   
        - [Generate API Key](#generate-api-key)
      - [Restaurants](#restaurants)
        - [List Restaurants](#list-restaurants)                  
        - [Create Restaurant](#new-restaurant)                   
        - [Update Restaurant](#update-restaurant)                
        - [Get Restaurant](#get-restaurant)                      
        - [Delete Restaurant](#delete-restaurant)                
        - [List Restaurants Near Me](#restaurants-near-me)       
        - [Search Restaurants](#search-restaurants)              
        - [List Restaurants By Owner](#list-restaurants-by-owner)
        - [Create Restaurant Menu](#create-restaurant-menu)
        - [List Restaurant Menus](#list-restaurant-menus)
        - [Get Restaurant QR Code](#get-restaurant-qr-code)
      - [Menu](#menu)
        - [Ask About Menu](#ask-about-menu)
        - [Get Menu](#get-menu) 
        - [Delete Menu](#delete-menu)
        - [Create Menu Group*](#create-menu-group)
        - [Get Menu's Group By id*](#create-menu-group)
      - [Menu Group](#menu-group)
        - [Get Menu Group By id](#get-menu-group-by-id)
        - [Update Menu Group](#update-menu-group)
        - [Delete Menu Group](#delete-menu-group)
        - [Create Menu Group's Menu Item*](#create-menu-item)
      - [Menu Item](#menu-item)
        - [Get Menu Item](#get-menu-item)
        - [Update Menu Item](#update-menu-item)

<!-- /TOC -->
<!-- Generated with https://marketplace.visualstudio.com/items?itemName=AlanWalk.markdown-toc -->

# WhatsAMenu API

**WhatsAMenu** is a food/drinks restaurant menu API allowing consumers to be as verbose as possible about their 
food items and drinks. It allows you to specify **categories**, **ingredients**, **allergens**. It 
even allows you to add images for menu items (dish) and each ingredient for the dish.

This API only returns data in a **JSON** format. It also accepts data in **JSON** format, unless otherwise specified.

### Supported Enums

#### Provinces 

| Province      | Description                         |
|---------------|-------------------------------------|
| EASTERN_CAPE  | Eastern Cape                        |
| WESTERN_CAPE  | Western Cape                        |
| NORTHERN_CAPE | Northern Cape                       |
| NORTH_WEST    | North West                          |
| GAUTENG       | Gauteng                             |
| MPUMALANGA    | Mpumalanga                          |
| LIMPOMPO      | Limpopo                             |
| FREE_STATE    | Free State                          |
| KWAZULU_NATAL | KwaZulu-Natal                       |

#### Upload Type Entities

| Entities   | Description |
|------------|-------------|
| RESTAURANT | Restaurant  |
| MENUITEM   | Menu Item   |
| INGREDIENT | Ingredient  |


#### HTTP Status Codes

These are the only response status codes to be expected from this API

| Status                      |
|-----------------------------|
| [200] - Ok                  | 
| [201] - Created             | 
| [400] - BadRequest          | 
| [401] - Unauthorized        | 
| [403] - Forbidden           | 
| [500] - InternalServerError | 

## Authentication
- Bearer 
- API Key

**WhatsAMenu API** uses either a **Bearer** token or the **API Key**. You receive a bearer token when you log into 
the system. You need the bearer token to generate the initial API key. Any subsequent keys can be generated using 
a bearer token or an API key.

### Accepted Headers

The Bearer token is passed through the `Authorization` header. And the API key can **only** be passed through the
`X-API-Key` header.

| Key           | Allowed                                                                    |
|---------------|----------------------------------------------------------------------------|
| Authorization | Bearer Token                                                               |
| X-API-Key     | API Key                                                                    |
| Content-Type  | - `application/json` <br/> - `multipart/form-data` <br/> - `text/event-stream` |


## API


These are all the version 1 `/v1/` endpoints available to manage restaurants, accounts and menu.

| Action                                                      | Method | Resource                                                        | Current Status |
|-------------------------------------------------------------|--------|-----------------------------------------------------------------|----------------|
| [Create New User Account](#new-user-account)                | POST   | v1/users/sign-up                                                | `DONE`         |
| [User Login](#login-user)                                   | POST   | v1/users/sign-in                                                | `DONE`         |
| [Generate API Key](#generate-api-key)                       | POST   | v1/users/api-key                                                | `DONE`         |
| [List Restaurants](#list-restaurants)                       | GET    | v1/restaurants                                                  | `IN PROGRESS`  |
| [Create Restaurant](#new-restaurant)                        | POST   | v1/restaurants                                                  | `DONE`         |
| [Update Restaurant](#update-restaurant)                     | PATCH  | v1/restaurants                                                  | `IN PROGRESS`  |
| [Get Restaurant](#get-restaurant)                           | GET    | v1/restaurants/**{id}**                                         | `DONE`         |
| [List Restaurant Menus](#list-restaurant-menus)             | GET    | v1/restaurants/**{id}**/menus                                   | `DONE`         |
| [Delete Restaurant](#delete-restaurant)                     | DELETE | v1/restaurants/**{id}**                                         | `IN PROGRESS`  |
| [List Restaurants Near Me](#restaurants-near-me)            | POST   | v1/restaurants/near-me                                          | `DONE`         |
| [Search Restaurants](#search-restaurants)                   | GET    | v1/restaurants/search?query=**{search term}**&limit=**{limit}** | `DONE`         |
| [List Restaurants By Owner](#list-restaurants-by-owner)     | GET    | v1/restaurants/owner                                            | `DONE`         |
| [Create Restaurant Menu](#create-restaurant-menu)           | POST   | v1/restaurants/**{id}**/menu                                    | `DONE`         |
| [Get Restaurant QR Code](#get-restaurant-qr-code)           | GET    | v1/restaurants/**{id}**/qrcode                                  | `DONE`         |
| [Get Menu](#get-menu)                                       | GET    | v1/menu/**{id}**                                                | `DONE`         |
| [Ask About Menu](#ask-about-menu)                           | GET    | v1/menu/enquire?menuId={menuId}&userId={userId}&prompt={query}  | `IN PROGRESS`  |
| [Delete Menu](#delete-menu)                                 | DELETE | v1/menu/**{id}**                                                | `DONE`         |
| [Create Menu Group](#create-menu-group)                     | POST   | v1/menu/**{id}**/menu-group                                     | `DONE`         |
| [List Menu Groups](#list-menu-groups)                       | GET    | v1/menu/**{id}**/menu-group                                     | `DONE`         |
| [Update Menu Group](#update-menu-group)                     | PATCH  | v1/menu-group/**{id}**                                          | `DONE`         |
| [Delete Menu Group](#delete-menu-group)                     | DELETE | v1/menu-group/**{id}**                                          | `DONE`         |
| [Create Grouped Menu Item](#create-grouped-menu-item)       | POST   | v1/menu-group/**{id}**/menu-items                               | `DONE`         |
| [List Grouped Menu Items](#list-grouped-menu-items)         | GET    | v1/menu-group/**{id}**/menu-items                               | `DONE`         |
| [Get Menu Item](#get-menu-item)                             | GET    | v1/menu-item/**{id}**                                           | `DONE`         |
| [Update Menu Item](#update-menu-item)                       | PATCH  | v1/menu-item/**{id}**                                           | `DONE`         |
| [Delete Menu Item](#delete-menu-item)                       | DELETE | v1/menu-item/**{id}**                                           | `IN PROGRESS`  |
| [Create Menu Item Allergen](#create-menu-item-allergen)     | POST   | v1/menu-item/**{id}**/allergens                                 | `DONE`         |
| [List Menu Item Allergens](#list-menu-item-allergens)       | GET    | v1/menu-item/**{id}**/allergens                                 | `DONE`         |
| [Delete Menu Item Allergen](#delete-menu-item-allergen)     | DELETE | v1/menu-item/**{id}**/allergens/**{allergenId}**                | `IN PROGRESS`  |
| [Create Menu Item Ingredient](#create-menu-item-ingredient) | POST   | v1/menu-item/**{id}**/ingredients                               | `DONE`         |
| [List Menu Item Ingredients](#list-menu-item-ingredients)   | GET    | v1/menu-item/**{id}**/ingredients                               | `DONE`         |
| [Update Ingredient](#update-ingredient)                     | PATCH  | v1/ingredients/**{id}**                                         | `DONE`         |
| [Delete Ingredient](#delete-ingredient)                     | DELETE | v1/ingredients/**{id}**                                         | `IN PROGRESS`  |
| [List Allergens](#list-allergens)                           | GET    | v1/allergens                                                    | `DONE`         |
| [Get Allergen](#get-allergen)                               | GET    | v1/allergens/**{id}**                                           | `DONE`         |
| [Upload Image Asset](#upload-image-asset)                   | PUT    | v1/upload                                                       | `DONE`         |

## Users

### New User Account

Signing up for a new account

#### Auth

- Anonymous

#### Request

`POST v1/users/sign-up`

#### Example body

```json
{
  "email": "sbu@example.com",
  "password": "#Sbu2@3$"
}
```

#### Example responses

`[201 - Created]`

```json
{
    "data": "user account created"
}
```

`[400 - Bad Request]`

```json
{
  "error": "Email already exists"
}
```


### Login User

Login user into the API

#### Auth

- Anonymous

#### Request

`POST v1/users/sign-in`

#### Example body

```json
{
  "email": "sbu@example.com",
  "password": "#Sbu2@3$"
}
```

#### Example responses

`[200 - OK]`

```json
{
  "token": "ey.............."
}
```

`[401 - Unauthorized]`

```json
{
  "error": "incorrect email or password"
}
```


### Generate API Key

Generates a new API Key. **WARN**: a user and API key exist in a **one-to-one** relationship. 
Generating a new key invalidates any previously existing key for a user.

#### Auth

- Bearer

#### Request

`POST v1/users/api-key`

#### Example

There is **no request body required** to generate a new key

#### Responses

`[200 - OK]`

```json
{
  "accessKey": "400762a01b9a0f18516fd001ea802178471e1594...."
}
```

`[400 - Bad Request]`

`[500 - InternalServerError]`

```json
{
  "error": "Cannot perform this action at this time"
}
```


## Restaurants

Endpoints to manage, search and filter restaurants

### New Restaurant

Creates a new restaurant

#### Auth

- API Key

#### Request

`POST - v1/restaurants`

**JSON BODY**

Consists of restaurant information, and it's address including geo-coordinates (used to locate restaurant)

| Key       | Required | Description                      |
|:----------|:---------|:---------------------------------|
| line1     | true     | Address Line 1                   |
| line2     | false    | Optional Address Line 2          |
| city      | true     | City                             |
| state     | true     | Province (check supported enums) |
| country   | false    | Optional country                 |
| latitude  | true     | Latitude                         |
| longitude | true     | Longitude                        |
| owner     | true     | Owner User Id                    |
| name      | true     | Restaurant name                  |
| summary   | true     | Short restaurant headline        |


#### Example

```json
{
  "line1": "311 Peter Mokaba Rd",
  "line2": "Morningside",
  "city": "Durban",
  "state": "KWAZULU_NATAL",
  "country": "South Africa",
  "latitude": 12.34385833,
  "longitude": -13.238437,
  "name": "Kota Place Restaurant",
  "owner": "451",
  "summary": "We sell the best kotas in Morningside"
}
```

#### Responses

`[201 - Created]`

```json
{
  "data": "restaurant created"
}
```

`[401 - Unauthorized]`

```json
{
  "error": "unable to get user claims"
}
```

`[400 - Bad Request]`

`[500 - InternalServerError]`

```json
{
  "error": "validationError - missing required field"
}
```



### Update Restaurant

Updates an existing restaurant details

#### Auth

- API key

#### Request

`PATCH - v1/restaurants/{restaurantId}`

**JSON BODY**


| Key       | Required | Description                      |
|:----------|:---------|:---------------------------------|
| line1     | true     | Address Line 1                   |
| line2     | false    | Optional Address Line 2          |
| city      | true     | City                             |
| state     | true     | Province (check supported enums) |
| country   | false    | Optional country                 |
| latitude  | true     | Latitude                         |
| longitude | true     | Longitude                        |
| name      | true     | Restaurant name                  |
| summary   | true     | Short restaurant headline        |


#### Example

```json
{
  "line1": "311 Peter Mokaba Rd",
  "line2": "Morning side",
  "city": "Durban",
  "state": "KWAZULU_NATAL",
  "country": "South Africa",
  "latitude": 12.34385833,
  "longitude": -13.238437,
  "name": "Kota Place",
  "summary": "We sell the best kotas in Durban"
}
```

#### Responses

`[200 - Ok]`

```json
{
  "data": "restaurant updated"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

`[400 - Bad Request]`

```json
{
  "error": "validationError - missing required field"
}
```

`[500 - InternalServerError]`

```json
{
  "error": "An error has occurred while updating restaurant"
}
```


### List Restaurants

`TODO`: refactor

Returns a list of unordered restaurants

#### Auth

- Anonymous

#### Request

`GET v1/restaurants`

#### Example responses


`[200 - OK]`

```json
[
  {
    "restaurantId": "134",
    "name": "SALT Morningside",
    "summary": "We make the best burgers in Morningside",
    "distance": null,
    "imageUrl": "https://b.zmtcdn.com/data/pictures/chains/2/7800592/6daa96e6.jpg",
    "address": {
      "addressId": "1",
      "line1": "Florida Morningside",
      "line2": "311 Peter Mokaba Road",
      "city": "Durban",
      "state": "KWAZULU_NATAL",
      "country": "South Africa",
      "latitude": 12.088,
      "longitude": 12.088
    },
    "updated": "",
    "created": ""
  }
]
```

`[400 - Bad Request]`

`[500 - InternalServerError]`

```json
{
  "error": "an unexpected error has occurred while processing request"
}
```

### Get Restaurant

Gets a restaurant by its id

#### Auth

- Anonymous

#### Request

`GET v1/restaurants/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "restaurantId": "134",
  "name": "SALT Morningside",
  "summary": "We make the best burgers in Morningside",
  "distance": null,
  "imageUrl": "https://b.zmtcdn.com/data/pictures/chains/2/7800592/6a3e6.jpg",
  "address": {
    "addressId": "1",
    "line1": "Florida Morningside",
    "line2": "311 Peter Mokaba Road",
    "city": "Durban",
    "state": "KWAZULU_NATAL",
    "country": "South Africa",
    "latitude": 12.088,
    "longitude": 12.088
  },
  "updated": "",
  "created": ""
}
```

`[400 - Bad Request]`

`[500 - InternalServerError]`

```json
{
  "error": "an unexpected error has occurred while processing request"
}
```


### Restaurants Near Me

Lists restaurants found near specified geolocation coordinates. The `distance` is in meters

#### Auth

- Anonymous

#### Request

`POST v1/restaurants/near-me`

#### Example

**JSON BODY**

```json
{
  "latitude": -29.791040,
  "longitude": 31.028410
}
```

#### Responses

`[200 - OK]`

```json
[
  {
    "restaurantId": "14",
    "name": "The Ocean Terrace - The Oyster Box Hotel",
    "summary": null,
    "distance": 8.89917396396982,
    "imageUrl": "https://b.zmtcdn.com/data/pictures/5/7800495/1e243dd4b12a2d9a26.jpg",
    "address": {
      "addressId": "33",
      "line1": "Umhlanga",
      "line2": "Oyter Box Hotel",
      "city": "Durban",
      "state": "KwaZulu-Natal",
      "country": "",
      "latitude": -29.727597,
      "longitude": 31.087159
    },
    "updated": "",
    "created": ""
  }
]
```

`[400 - Bad Request]`

`[500 - InternalServerError]`

```json
{
  "error": "an unexpected error has occurred while processing request"
}
```


### Delete Restaurant

Marks a restaurant for deletion. **NB** Deleting a restaurant will also remove any items associated with it, 
such as a **menu**, **address**, **menu items**, **groups**, and **ingredients**.

#### Auth

- API Key

#### Request

`DELETE v1/restaurants/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "data": "item deleted"
}
```

`TODO`: Finish auth on this endpoint


### Search Restaurants

Search for restaurants by name

#### Auth

- Anonymous

#### Request

`GET v1/restaurants/search?query={searchTerm}&limit={limit}`

- Query Params

| Param | Required | Description                                                    |
|:------|:---------|:---------------------------------------------------------------|
| query | true     | Search query, i.e restaurant name. **NB**: length must be >= 3 |
| limit | false    | Number of results to be returned, **default**: 10              |


#### Example responses

`[200 - OK]`

```json
[
  {
    "restaurantId": "1",
    "name": "SALT Morningside",
    "summary": "We make the best burgers in Morningside",
    "distance": null,
    "imageUrl": "https://b.zmtcdn.com/data/pictures/2/3cbabb3a3e6.jpg",
    "address": {
      "addressId": "1",
      "line1": "Florida Morningside",
      "line2": "311 Peter Mokaba Road",
      "city": "Durban",
      "state": "KwaZulu-Natal",
      "country": "",
      "latitude": 12.088,
      "longitude": 12.088
    },
    "updated": "",
    "created": ""
  }
]
```

`[400 - Bad Request]`

```json
{
  "error": "missing required search input"
}
```

`[500 - InternalServerError]`

```json
{
  "error": "an unexpected error has occurred while processing request"
}
```


### List Restaurants By Owner

Get a list of restaurant by owner (current authenticated user)

#### Auth

- Bearer
- **or** API Key

#### Request

`GET v1/restaurants/owner`

#### Example responses

`[200 - OK]`

```json
[
  {
    "restaurantId": "61",
    "name": "Dukkah Florida",
    "summary": "Hey",
    "distance": null,
    "imageUrl": "",
    "address": {
      "addressId": "80",
      "line1": "Kensignton",
      "line2": "2nd Beach road",
      "city": "Durbz",
      "state": "KWAZULU_NATAL",
      "country": "",
      "latitude": 0.16364,
      "longitude": 1.92384872
    },
    "updated": "",
    "created": ""
  }
]
```

`[401 - Bad Request]`

```json
{
  "error": "cannot acquire user claims"
}
```

`[500 - InternalServerError]`

```json
{
  "error": "an unexpected error has occurred while processing request"
}
```


### Create Restaurant Menu

Creates a menu and associates it with a restaurant

#### Auth

- API Key

#### Request

`PATCH - v1/restaurants/{id}/menu`

**JSON BODY**


| Key       | Required | Description           |
|:----------|:---------|:----------------------|
| name      | true     | Menu name             |
| summary   | false    | Brief summary of menu |


#### Example

```json
{
  "name": "Steers Menu",
  "summary": "Ultimate menu"
}
```

#### Responses

`[201 - Created]`

```json
{
  "data": "menu created"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

`[400 - Bad Request]`

```json
{
  "error": "validationError - missing required field"
}
```

### List Restaurant Menus

Returns a list of restaurant menu, and no additional menu data such as menu items, groups or ingredients

#### Auth

- Anonymous

#### Request

`GET v1/restaurants/{id}/menus`

#### Example response

`[200 - Ok]`

```json
[
  {
    "menuId": "4",
    "name": "Butcher Boys Morningside",
    "summary": null,
    "restaurantId": "10",
    "menuGroups": null,
    "updated": "2023-04-09 15:38:59",
    "created": "2023-04-09 15:38:59"
  }
]
```


### Get Restaurant QR Code

Returns a QR Code image which is just a link to the restaurant's menu

#### Auth

- Anonymous

#### Request

`GET v1/restaurants/{id}/qrcode`

#### Example response

`[200 - Ok]`

```json
{
	"imageUri": "iVBORw0KGgoAAAANSUhEUgAAAQAAAAEAAQMAAABmvDolAAAABlB..."
}
```


## Menu

### Ask About Menu

Ask any questions related to the menu, such as allergens, food type, your budget etc. This uses an AI model 
underneath with the context of the menu

- Query Params

| Param  | Required | Description                                        |
|:-------|:---------|:---------------------------------------------------|
| userId | true     | The user's identifier, used to retain user context |
| menuId | true     | The menu in question                               |
| prompt | true     | The query about the menu                           |

#### Request

`GET v1/menu/enquire`


#### Example Response

This returns a stream of JSON responses. 
This is best used with an <a href="https://developer.mozilla.org/en-US/docs/Web/API/EventSource">EventSource</a>

| Data                         | Time         |
|:-----------------------------|:-------------|
| ```{"content":"For"}```      | 22:08:48.059 | 
| ```{"content":" a"}```       | 22:08:48.060 | 
| ```{"content":" seafood"}``` | 22:08:48.061 | 
| ```{"content":" option"}```  | 22:08:48.062 | 
| ```{"content":" within"}```  | 22:08:48.062 | 
| ```{"content":" a"}```       | 22:08:48.062 | 
| ```{"content":" R250"}```    | 22:08:48.063 | 
| ```{"content":" budget"}```  | 22:08:48.063 | 







### Create Menu Group

Creates a menu group/section/category within the main menu

#### Auth

- API Key

#### Request

`POST v1/menu/{id}/menu-group`

#### Example body

```json
{
  "name": "Beverages",
  "summary": "All you can drink"
}
```

#### Example responses

`[200 - OK]`

```json
{
  "data": "menu group created"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Get Menu

Returns the menu of the restaurant with all associated items

**Menu Structure**
- Menu
    - Menu Groups[]
        - Menu Items[]
            - Ingredients[]

#### Auth

- Anonymous

#### Request

`GET v1/menu/{id}`

#### Example response

`[200 - Ok]`
```json
{
  "menuId": "2",
  "name": "Dukkah Restaurant & Bar",
  "summary": "Dukkah Restaurant & Bar on Durban’s Florida road is a high point on the durban dining scene - a modern and luxuriant social hub with an upscale cocktail bar and lounge. ",
  "restaurantId": "8",
  "menuGroups": [
    {
      "menuId": "2",
      "menuGroupId": "9",
      "name": "Starters",
      "summary": "",
      "items": [
        {
          "menuItemId": "15",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Chicken Croquettes",
          "summary": "Thyme, anchovies, onion, garlic, mustard, chilli, Grana Padano, celery dressing",
          "description": "Thyme, anchovies, onion, garlic, mustard, chilli, Grana Padano, celery dressing",
          "imageUrl": "",
          "price": 110,
          "ingredients": [],
          "updated": "2023-04-03 20:37:23",
          "created": "2023-04-03 20:37:23"
        },
        {
          "menuItemId": "16",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Confit Caprese Salad",
          "summary": "Exotic tomatoes, orange segment, basil-mozzarella cheese",
          "description": "Exotic tomatoes, orange segment, basil-mozzarella cheese",
          "imageUrl": "",
          "price": 115,
          "ingredients": [],
          "updated": "2023-04-03 20:38:44",
          "created": "2023-04-03 20:38:44"
        },
        {
          "menuItemId": "18",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Suicide Wings",
          "summary": "With Blue Cheese Sauce",
          "description": "",
          "imageUrl": "",
          "price": 90,
          "ingredients": [],
          "updated": "2023-04-03 20:40:14",
          "created": "2023-04-03 20:40:14"
        },
        {
          "menuItemId": "19",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Thai Fish Cakes",
          "summary": "Thai style, panko, coriander, soy, chili, ginger",
          "description": "Thai style, panko, coriander, soy, chili, ginger",
          "imageUrl": "",
          "price": 110,
          "ingredients": [],
          "updated": "2023-04-03 20:41:30",
          "created": "2023-04-03 20:41:30"
        },
        {
          "menuItemId": "21",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Saute Chicken Livers",
          "summary": "Chorizo, chili flakes, creamy napoli",
          "description": "Chorizo, chili flakes, creamy napoli",
          "imageUrl": "",
          "price": 80,
          "ingredients": [],
          "updated": "2023-04-03 21:14:48",
          "created": "2023-04-03 20:52:12"
        },
        {
          "menuItemId": "22",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "Salt and Pepper Squid",
          "summary": "Deep fried, asian slaw, aioli",
          "description": "Deep fried, asian slaw, aioli",
          "imageUrl": "",
          "price": 95,
          "ingredients": [],
          "updated": "2023-04-03 20:54:54",
          "created": "2023-04-03 20:54:54"
        },
        {
          "menuItemId": "23",
          "menuId": "2",
          "menuGroupId": "9",
          "name": "West Coast Mussels",
          "summary": "Nappe of shallots, cider, bacon, thyme, ciabatta",
          "description": "Nappe of shallots, cider, bacon, thyme, ciabatta",
          "imageUrl": "",
          "price": 130,
          "ingredients": [],
          "updated": "2023-04-03 20:58:50",
          "created": "2023-04-03 20:58:50"
        }
      ],
      "updated": "2023-04-02 15:58:53",
      "created": "2023-04-02 15:58:53"
    },
    {
      "menuId": "2",
      "menuGroupId": "11",
      "name": "Pasta",
      "summary": "",
      "items": [
        {
          "menuItemId": "26",
          "menuId": "2",
          "menuGroupId": "11",
          "name": "Pulled Braised Short-Ribs Tagliatelle",
          "summary": "Grain mustard, home-made tagliatelle, cilantro, jus",
          "description": "Grain mustard, home-made tagliatelle, cilantro, jus",
          "imageUrl": "",
          "price": 170,
          "ingredients": [],
          "updated": "2023-04-04 18:10:34",
          "created": "2023-04-04 18:10:34"
        },
        {
          "menuItemId": "27",
          "menuId": "2",
          "menuGroupId": "11",
          "name": "Chili Prawn Fettuccine",
          "summary": "Napoli, coriander, grated pecorino",
          "description": "Napoli, coriander, grated pecorino",
          "imageUrl": "",
          "price": 175,
          "ingredients": [],
          "updated": "2023-04-04 18:11:12",
          "created": "2023-04-04 18:11:12"
        },
        {
          "menuItemId": "28",
          "menuId": "2",
          "menuGroupId": "11",
          "name": "Basil & Chicken Penne",
          "summary": "Pesto, pine kernels, shaved parmesan",
          "description": "Pesto, pine kernels, shaved parmesan",
          "imageUrl": "",
          "price": 145,
          "ingredients": [],
          "updated": "2023-04-04 18:12:01",
          "created": "2023-04-04 18:12:01"
        },
        {
          "menuItemId": "29",
          "menuId": "2",
          "menuGroupId": "11",
          "name": "Calamari & Chorizo Rigatoni",
          "summary": "Kalamata olives, charred red pepper, tomato salsa",
          "description": "Kalamata olives, charred red pepper, tomato salsa",
          "imageUrl": "",
          "price": 165,
          "ingredients": [],
          "updated": "2023-04-04 18:12:43",
          "created": "2023-04-04 18:12:43"
        },
        {
          "menuItemId": "30",
          "menuId": "2",
          "menuGroupId": "11",
          "name": "Seafood Linguini",
          "summary": "Chardonnay cream, prawns, mussels, squid, chili, gremolata",
          "description": "Chardonnay cream, prawns, mussels, squid, chili, gremolata",
          "imageUrl": "",
          "price": 195,
          "ingredients": [],
          "updated": "2023-04-04 18:13:39",
          "created": "2023-04-04 18:13:39"
        }
      ],
      "updated": "2023-04-02 15:59:27",
      "created": "2023-04-02 15:59:27"
    }
  ],
  "updated": "2023-04-02 15:37:51",
  "created": "2023-04-02 15:37:51"
}
```


### Delete Menu

Marks a menu for deletion. **NB** Deleting a menu will also remove any items associated with it,
such as **menu items**, **groups**, and **ingredients**.

#### Auth

- API Key

#### Request

`DELETE v1/menu/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "data": "item deleted"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

## Menu Group

### Update Menu Group

Updates a menu group

#### Auth

- API Key

#### Request

`PATCH v1/menu-group/{id}`

#### Example body

```json
{
  "name": "Meat",
  "summary": "All you can eat meat meals"
}
```

#### Example responses

`[200 - OK]`

```json
{
  "data": "menu group updated"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Delete Menu Group

Deletes a menu group and all menu items attached to it

#### Auth

- API Key

#### Request

`DELETE v1/menu-group/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "data": "item deleted"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### List Menu Groups

Lists all menu groups (categories) found under a menu

#### Auth

- Anonymous

#### Request

`GET v1/menu/{id}/menu-groups`

#### Example response

`[200 - Ok]`
```json
[
  {
    "menuId": "1",
    "menuGroupId": "10",
    "name": "Extra Value Meals",
    "summary": "Extra Value Meals",
    "items": null,
    "updated": "2022-01-08 12:21:43",
    "created": "2022-01-08 12:21:43"
  },
  {
    "menuId": "1",
    "menuGroupId": "11",
    "name": "Sides",
    "summary": "Sides",
    "items": null,
    "updated": "2022-01-08 12:21:43",
    "created": "2022-01-08 12:21:43"
  },
  {
    "menuId": "1",
    "menuGroupId": "12",
    "name": "Sharing",
    "summary": "Sharing",
    "items": null,
    "updated": "2022-01-08 12:21:43",
    "created": "2022-01-08 12:21:43"
  }
]
```

### List Grouped Menu Items

List all menu items under a menu group/category/section

#### Auth

- Anonymous

#### Request

`GET v1/menu-group/{id}/menu-items`

#### Example response

`[200 - OK]`

```json
[
  {
    "menuItemId": "46",
    "menuId": "18",
    "menuGroupId": "20",
    "name": "Ice Cream",
    "summary": "Dessert that will melt your taste away",
    "description": "Dessert that will melt your taste away",
    "imageUrl": "",
    "price": 13.65,
    "ingredients": null,
    "updated": "",
    "created": ""
  }
]
```

### Create Grouped Menu Item

Create a menu item under a group/category

#### Auth

- API Key

#### Request

`POST v1/menu-group/{id}/menu-items`

#### Example body

```json
{
  "name": "Medium Chips",
  "summary": "Get any two awesome burgers for the price of one",
  "description": "The wacky wednesday special gives you 2 chicken or beef",
  "price": 20.39,
  "allergens": ["3", "5"]
}
```

#### Example responses

`[200 - OK]`

```json
{
  "data": "menu item created"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

## Menu Item

### Get Menu Item

Gets a menu item by id

#### Auth

- Anonymous

#### Request

`GET v1/menu-item/{id}`

#### Example response

`[200 - Ok]`

```json
{
  "menuItemId": "10",
  "menuId": "1",
  "menuGroupId": "9",
  "name": "Another Item",
  "summary": "For those extra good mornings, start your day with a seasoned boerie patty, egg, ketchup and onion on a freshly toasted bun.",
  "description": "For those extra good mornings, start your day with a seasoned boerie patty, egg, ketchup and onion on a freshly toasted bun.",
  "imageUrl": "public/menu-items/1-b74b2b6acf4a14235ba2219b7511a34c.jpeg",
  "price": 3,
  "ingredients": null,
  "updated": "2022-03-06 17:45:33",
  "created": "2022-03-06 17:45:33"
}
```

### Update Menu Item

Update a menu item

#### Auth

- API key

#### Request

`PATCH v1/menu-item/{id}`

#### Example body

```json
{
	"name": "Nice Ice Cream",
	"summary": "Best ice cream in the Milky-Way",
	"description": "Melting ice-cream",
	"price": 20.39,
	"menuGroupId": 20
}
```

#### Example responses

`[200 - OK]`

```json
{
  "data": "menu item updated"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Delete Menu Item

Marks a menu item for deletion

#### Auth

- API Key

#### Request

`DELETE v1/menu-item/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "data": "item deleted"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Create Menu Item Ingredient

Creates/adds an allergen to a menu item

#### Auth

- API Key

#### Request

`POST v1/menu-item/{id}/ingredients`

#### Example body

```json
{
  "name": "Tomato"
}
```

#### Example response

`[201 - Created]`

```json
{
	"data": "ingredient created"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Create Menu Item Allergen

Creates/adds an allergen to a menu item

#### Auth

- API Key

#### Request

`POST v1/menu-item/{id}/allergens`

#### Example body

```json
{
	"allergenId": "7"
}
```

#### Example responses

`[201 - Created]`

```json
{
  "data": "allergen added"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

`[500 - InternalServerError]`

```json
{
	"error": "MenuItem or Allergen was not found"
}
```

### List Menu Item Ingredients

List all ingredients of a menu item

#### Auth

- Anonymous

#### Request

`GET v1/menu-item/{id}/ingredients`

#### Example response

`[200 - OK]`

```json
[
  {
    "ingredientId": "1",
    "menuItemId": "1",
    "name": "Regular Bun",
    "imageUrl": "https://www.mcdonalds.co.za/media/Ingredients/regular.png",
    "updated": "2022-01-08 12:37:50",
    "created": ""
  },
  {
    "ingredientId": "2",
    "menuItemId": "1",
    "name": "Ketchup",
    "imageUrl": "https://www.mcdonalds.co.za/media/Ingredients/ketchup.png",
    "updated": "2022-01-08 12:37:50",
    "created": ""
  }
]
```

### List Menu Item Allergens

Lists all allergens for a menu item

#### Auth

- Anonymous

#### Request

`GET v1/menu-item/{id}/allergens`

#### Example response

```json
[
  {
    "allergenId": "2",
    "menuItemId": "16",
    "name": "Nuts",
    "summary": "Contains nuts",
    "updated": "",
    "created": ""
  }
]
```

## Ingredient

### Update Ingredient

Updates an ingredient of a menu item

#### Auth

- API Key

#### Request

`PATCH v1/ingredients/{id}`

#### Example body

```json
{
  "name": "Cheese",
  "menuItemId": 542
}
```

#### Example responses

`[200 - OK]`

```json
{
  "data": "ingredient updated"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

### Delete Ingredient

Marks an ingredient for deletion

#### Auth

- API Key

#### Request

`DELETE v1/ingredients/{id}`

#### Example responses

`[200 - OK]`

```json
{
  "data": "ingredient deleted"
}
```

`[403 - Forbidden]`

```json
{
  "error": "explicit deny: user not permitted to modified resource"
}
```

## Allergen

### Get Allergen

Looks up an allergen by its id

#### Auth

- Anonymous

#### Request

`GET v1/allergens/{id}`

#### Example response

`[200 - Ok]`
```json
{
  "allergenId": "2",
  "name": "Nuts",
  "summary": "Nuts",
  "updated": "",
  "created": ""
}
```


### List Allergens

Returns a list of all allergens

#### Auth

- Anonymous

#### Request

`GET v1/allergens`

#### Example response

`[200 - Ok]`
```json
[
  {
      "allergenId": "1",
      "name": "Wheat",
      "summary": "Wheat",
      "updated": "",
      "created": ""
  },
  {
      "allergenId": "2",
      "name": "Nuts",
      "summary": "Nuts",
      "updated": "",
      "created": ""
  }
]
```


## Uploads

### Upload Image Asset

Single unified resource for uploading **menu** and **restaurant** related images

### Image Criteria
- PNG / JPG / JPEG
- < 1MB

#### Auth

- API Key

#### Request

`PUT v1/upload`

#### Body

Content-Type: `multipart/form-data`


| Key        | Required | Type                                  |
|------------|----------|---------------------------------------|
| entityType | true     | [Entity Types](#upload-type-entities) |             
| entityId   | true     | int/string                            |             
| fileData   | true     | file                                  |

#### Example response

`[200 - OK]`

```json
{
  "data": "public/menu-items/5-b2a7f1c6be9edf1ac591c123b6ed2f90.jpg"
}
```

`[400 - BadRequest]`

```json
{
  "error": "image too large"
}
```