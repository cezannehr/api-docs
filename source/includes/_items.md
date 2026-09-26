# Items

An item in Learn Amp is a learning object. It could be a SCORM, xAPI or AICC package. It could be a URL link to an external web page. It could be an embedded video, or downloadable file.

Items are typically organised into Channels and Learnlists.

## View all Items

> View all items in your account:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/items' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all
      response = self.class.get("/items", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

items = Learnamp::Items.new(token).all
```

View all items

`GET https://{API_BASE_URL}/v1/items`

### Required Scope
This endpoint requires the `items:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "items": [
        {
            "id": 3015,
            "title": "Activity 10",
            "shortDescription": null,
            "itemType": "Video",
            "itemCategory": "Audio/Visual",
            "itemUrl": "https://examplecompany.learnamp.com/en/items/activity-10",
            "createdAt": "2021-03-02T11:09:35Z",
            "updatedAt": "2021-03-02T12:24:02Z",
            "addedBy": {
                "id": 1
            },
            "tags": ["marketing", "video"]
        },
        {
            "id": 3014,
            "title": "rust-lang/rust",
            "shortDescription": null,
            "itemType": "Other",
            "itemCategory": "Other",
            "itemUrl": "https://examplecompany.learnamp.com/en/items/rust-lang-rust",
            "createdAt": "2021-03-01T09:15:00Z",
            "updatedAt": "2021-03-01T09:15:00Z",
            "addedBy": {
                "id": null
            },
            "tags": []
        }
    ]
}

```

Each item includes `createdAt`, `updatedAt`, `addedBy` and `tags`. `addedBy` contains only the user `id` who added the item. When an item was added by an external contributor, `addedBy.id` is `null` — the full contributor details are available on the [single item endpoint](#show-an-item).

## Show an Item

> Display details for a single Item:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/items/3015' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/items/#{id}", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

item = Learnamp::Items.new(token).find(3015)
```

Display all details for one specific Item.

`GET https://{API_BASE_URL}/v1/items/{itemId}`

### Required Scope
This endpoint requires the `items:read` scope.

> 200 OK - successful response:

```json
{
    "id": 3015,
    "title": "Marketing 101",
    "shortDescription": null,
    "itemType": "Video",
    "itemCategory": "Audio/Visual",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/marketing-101",
    "url": "https://www.youtube.com/watch?v=8Sj2tbh-ozE",
    "description": "<p>Testing Youtube video</p>",
    "slug": "marketing-101",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-02T11:09:35Z",
    "updatedAt": "2021-03-02T12:24:02Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1614683346/c6q5ylwoencf2iaoc4zs.jpg",
    "supplier": {
        "id": 138,
        "name": "Youtube.com",
        "logo": "https://examplecompany.learnamp.com/assets/placeholders/organisation_avatar-cd6e9138f99068ded8cca1210674ac7abd1329738317547968943addc6056ae7.png"
    },
    "addedBy": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Tester",
        "email": "test@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "hireDate": "2017-02-01",
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 10 mins",
    "tags": ["marketing", "video"],
    "skills": ["marketing", "video"]
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## List Learnlists Containing an Item

> List the learnlists that contain a single item:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/items/3015/learnlists' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def learnlists(id, filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/items/#{id}/learnlists?#{filters_query}", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

learnlists = Learnamp::Items.new(token).learnlists(3015)
```

List all of the learnlists that contain a given item, most recently created first.

`GET https://{API_BASE_URL}/v1/items/{itemId}/learnlists`

### Required Scope
This endpoint requires the `items:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "learnlists": [
        {
            "id": 379,
            "name": "Test learnlist",
            "description": "This is the learnlist description"
        }
    ]
}

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## List Channels Containing an Item

> List the channels that contain a single item:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/items/3015/channels' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def channels(id, filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/items/#{id}/channels?#{filters_query}", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

channels = Learnamp::Items.new(token).channels(3015)
```

List all of the channels that contain a given item, most recently created first.

`GET https://{API_BASE_URL}/v1/items/{itemId}/channels`

### Required Scope
This endpoint requires the `items:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "channels": [
        {
            "id": 42,
            "title": "Onboarding"
        }
    ]
}

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Create an Item

> Create a new Item:

```shell
curl --location --request POST 'https://{API_BASE_URL}/v1/items' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'url=https://www.test.com' \
--form 'description=This is the description' \
--form 'title=This is Item title' \
--form 'itemType=video' \
--form 'totalTime=less_than_one_hour'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/items", { body: params, headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

params = {
  title: "Sales",
  url: "https://www.test.com",
  description: "This is the description",
  title: "This is Item title",
  itemType: "video",
  totalTime: "less_than_one_hour"
}

item = Learnamp::Items.new(token).create(params)
```

Create an Item

`POST https://{API_BASE_URL}/v1/items`

### Required Scope
This endpoint requires the `items:create` scope.

### Data in Body

Parameter (* required) | Example value | Description
--------- | ------- | -----------
title * | Sales and Marketing Guide | Title of item
url | https://www.test.com | URL to external web page for the item
description | Some description text | Description of the item
expires | true | If item must expire, set to true
expiresAt | 2022-12-31 | Expiry date when item should expire in ISO 8601 format
goesLive | true | If item should have a future go live date
goesLiveAt | 2022-12-31 | Future go live date in ISO 8601 format
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
visibility | entire_company | Item visibility. Must be one of "hidden", "selected", "entire_company"
sourceType | Udemy | Short string identifier of the source of the item, if is coming from an external LMS or other system
sourceId | e814koip | External identifier of item, if it if coming from an external LMS or other system
itemType | article | Item type, one of: "article", "document", "book", "q_and_a", "info_card", "author_content", "video", "elearning", "infographic", "audio", "slides", "image", "project", "classroom", "coaching", "course", "event", "presentation", "meeting", "webinar", "action", "test", "qa", "announcement", "newsletter", "reminder", "website", "other"
totalTime | less_than_one_hour | Approximate time to complete. One of: "less_than_fifteen_minutes", "less_than_one_hour", "one_to_ten_hours", "ten_to_one_hundred_hours", "more_than_one_hundred_hours", "less_than_five_minutes", "five_to_ten_minutes", "ten_to_twenty_minutes", "twenty_to_thirty_minutes", "more_than_thirty_minutes", "less_than_ten_minutes", "ten_to_thirty_minutes", "thirty_minutes_to_one_hour", "one_to_two_hours", "more_than_two_hours", "two_to_four_hours", "four_to_six_hours", "more_than_six_hours", "less_than_two_hours", "four_to_eight_hours", "eight_to_twelve_hours", "more_than_twelve_hours", "more_than_one_hour", "custom_time"
itemCategory | audiovisual | Category. One of: "written", "audiovisual", "activity_category", "assessment", "update_category", "other_category"
tags | marketing,sales,onboarding | Assign tags to the created item
skills | leadership,communication | Assign skills to the created item
externallyControlledCompletion | true | Prevent automatic or manual completion in app. Used in conjunction with the activities creation API to control completion.



### Source Type & ID

The above mentioned parameters `sourceType` and `sourceId` are used to link the item to an external system. The API will prevent creation of items with a colliding combination of `sourceType` and `sourceId` within the context of your company account. Duplication between e.g. live and test accounts is permitted.

> 201 Created - successful response:

```json
{
    "id": 3016,
    "title": "new item with tags",
    "shortDescription": null,
    "itemType": "Other",
    "itemCategory": "Other",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/new-item-with-tags",
    "url": null,
    "description": "Short desc",
    "slug": "new-item-with-tags",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-26T17:29:25Z",
    "updatedAt": "2021-03-26T17:29:25Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1588665654/e5pclrrfd8xyyffphich.jpg",
    "supplier": null,
    "addedBy": {
        "id": 1381,
        "firstName": "Rebecca",
        "lastName": "Bridger",
        "jobTitle": "Founder & Managing Director",
        "email": "rebecca@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "owner",
        "hireDate": null,
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1381",
        "status": {
            "status": "Not yet invited"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 15 mins",
    "tags": [],
    "skills": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "title is missing",
    "fullErrors": {
        "title": [
            "is missing"
        ],
    }
}
```

> 400 Bad request - duplicate source item:

```json
{
    "error": "Item with source_type 'Integration for Sheffield University' and source_id 'test123' already exists",
    "fullErrors": {
        "base": [
            "Item with source_type 'Integration for Sheffield University' and source_id 'test123' already exists"
        ]
    }
}
```

## Update an Item

> Update an Item:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/items/383' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'title=New Item Title'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def update(id, params)
      response = self.class.put("/items/#{id}", { body: params, headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

item = Learnamp::Items.new(token).update(1234, params)
```

Update an Item

`PUT https://{API_BASE_URL}/v1/items/{itemId}`

### Required Scope
This endpoint requires the `items:update` scope.

### Data in Body

Parameter (* required) | Example value | Description
--------- | ------- | -----------
title * | Sales and Marketing Guide | Title of item
url | https://www.test.com | URL to external web page for the item
description | Some description text | Description of the item
expires | true | If item must expire, set to true
expiresAt | 2022-12-31 | Expiry date when item should expire in ISO 8601 format
goesLive | true | If item should have a future go live date
goesLiveAt | 2022-12-31 | Future go live date in ISO 8601 format
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
visibility | entire_company | Item visibility. Must be one of "hidden", "selected", "entire_company"
sourceType | Udemy | Short string identifier of the source of the item, if is coming from an external LMS or other system
sourceId | e814koip | External identifier of item, if it if coming from an external LMS or other system
itemType | article | Item type, one of: "article", "document", "book", "q_and_a", "info_card", "author_content", "video", "elearning", "infographic", "audio", "slides", "image", "project", "classroom", "coaching", "course", "event", "presentation", "meeting", "webinar", "action", "test", "qa", "announcement", "newsletter", "reminder", "website", "other"
totalTime | less_than_one_hour | Approximate time to complete. One of: "less_than_fifteen_minutes", "less_than_one_hour", "one_to_ten_hours", "ten_to_one_hundred_hours", "more_than_one_hundred_hours", "less_than_five_minutes", "five_to_ten_minutes", "ten_to_twenty_minutes", "twenty_to_thirty_minutes", "more_than_thirty_minutes", "less_than_ten_minutes", "ten_to_thirty_minutes", "thirty_minutes_to_one_hour", "one_to_two_hours", "more_than_two_hours", "two_to_four_hours", "four_to_six_hours", "more_than_six_hours", "less_than_two_hours", "four_to_eight_hours", "eight_to_twelve_hours", "more_than_twelve_hours", "more_than_one_hour", "custom_time"
itemCategory | audiovisual | Category. One of: "written", "audiovisual", "activity_category", "assessment", "update_category", "other_category"
archive | true | If item must be archived, set to true. To unarchive an item set to false.

> 200 OK - successful response:

```json
{
    "id": 3016,
    "title": "new item with tags",
    "shortDescription": null,
    "itemType": "Other",
    "itemCategory": "Other",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/new-item-with-tags",
    "url": null,
    "description": "Short desc",
    "slug": "new-item-with-tags",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-26T17:29:25Z",
    "updatedAt": "2021-03-26T17:29:25Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1588665654/e5pclrrfd8xyyffphich.jpg",
    "supplier": null,
    "addedBy": {
        "id": 1381,
        "firstName": "Rebecca",
        "lastName": "Bridger",
        "jobTitle": "Founder & Managing Director",
        "email": "rebecca@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "owner",
        "hireDate": null,
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1381",
        "status": {
            "status": "Not yet invited"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 15 mins",
    "tags": [],
    "skills": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "title is missing",
    "fullErrors": {
        "title": [
            "is missing"
        ]
    }
}
```

## Complete an Item

> Mark an item as completed for a user:

```shell
# Complete by item ID
curl --location --request POST 'https://{API_BASE_URL}/v1/items/complete' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--data-raw '{
  "itemId": 3015,
  "userId": 123
}'

# Complete by source type and ID
curl --location --request POST 'https://{API_BASE_URL}/v1/items/complete' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--data-raw '{
  "sourceType": "Integration for Sheffield University",
  "sourceId": "test123",
  "email": "user@example.com"
}'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def complete(params)
      response = self.class.post("/items/complete", { body: params, headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

# Complete by item ID
params = { itemId: 3015, userId: 123 }
activity = Learnamp::Items.new(token).complete(params)

# Complete by source type and ID
params = {
  sourceType: 'Integration for Sheffield University',
  sourceId: 'test123',
  email: 'user@example.com'
}
activity = Learnamp::Items.new(token).complete(params)
```

### HTTP Request
`POST https://{API_BASE_URL}/v1/items/complete`

### Required Scope
This endpoint requires the `items:complete` scope.

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
itemId | Optional* | The ID of the item in LearnAmp
sourceType | Optional* | The type/name of the integration or source
sourceId | Optional* | The ID of the item in the source system
userId | Optional** | The ID of the user to mark completion for
email | Optional** | The email of the user to mark completion for

\* Either `itemId` OR both `sourceType` and `sourceId` must be provided
\** Either `userId` OR `email` must be provided

### Response

The response will be an Activity object representing the completion.

> Example Response:

```json
{
  "id": 12345,
  "verb": "completed",
  "completed": true,
  "createdAt": "2024-03-20"
}
```

### Error Responses

> 400 Bad Request - validation errors:

```json
{
  "error": "When identifying an item by source, both sourceType and sourceId must be provided"
}
```

```json
{
  "error": "userId, email are missing, exactly one parameter must be provided"
}
```

```json
{
  "error": "itemId, sourceType are mutually exclusive"
}
```

> 404 Not Found - when user or item not found:

```json
{
  "error": "Couldn't find User"
}
```

```json
{
  "error": "Couldn't find Item"
}
```

> 403 Forbidden - when token lacks required scope:

```json
{
  "error": "Access to this resource requires scope \"items:complete\"."
}
```

## Delete an Item

> Delete an item:

```shell
curl --location --request DELETE 'https://{API_BASE_URL}/v1/items/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def delete(id)
      response = self.class.delete("/items/#{id}", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

Learnamp::Items.new(token).delete(1)
```

Delete an Item

`DELETE https://{API_BASE_URL}/v1/items/{itemId}`

### Required Scope
This endpoint requires the `items:delete` scope.

> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
