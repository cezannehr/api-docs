# Learnlists

## Learnlist Description

In Learn Amp, a Learnlist is a 'playlist' of content. It can be structured, like a course with a specific sequence, or an unordered collection of learning items.

## View All Learnlists

> View all learnlists in your account:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/learnlists?#{filters_query}", { headers: headers })
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

filters = {
}
teams = Learnamp::Learnlists.new(token).all(filters)
```

View all learnlists

`GET https://{API_BASE_URL}/v1/learnlists`

### Required Scope
This endpoint requires the `learnlists:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "learnlists": [
        {
            "id": 379,
            "name": "Test learnlist",
            "description": "This is the learnlist description"
        },
  ]
}

```

## Show a Learnlist

> Display details for a single Learnlist:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists/379' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/learnlists/#{id}", { headers: headers })
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

learnlist = Learnamp::Learnlists.new(token).find(379)
```

Display details for one specific Learnlist.

`GET https://{API_BASE_URL}/v1/learnlists/{learnlistId}`

### Required Scope
This endpoint requires the `learnlists:read` scope.

> 200 OK - successful response:

```json
{
    "id": 379,
    "name": "Test learnlist",
    "description": "This is the learnlist description"
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## List a Learnlist's Contents

> List the contents of a single Learnlist:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists/379/contents' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def contents(id, filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/learnlists/#{id}/contents?#{filters_query}", { headers: headers })
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

contents = Learnamp::Learnlists.new(token).contents(379)
```

List all of a Learnlist's contents, in Learnlist order.

A Learnlist can contain more than just Items — it may also hold Quizzes, Surveys and Events. This endpoint returns every entry, regardless of type. Each entry carries a `type` discriminator (`Item`, `Quiz`, `Survey` or `Event`) which you can pair with the entry `id` to fetch type-specific detail from the relevant endpoint (e.g. `GET /v1/items/:id`).

`displayType` is the product-facing label for the entry type. For Learnlist contents it always mirrors `type`; it differs only for content types whose internal name differs from the name shown in the product.

`GET https://{API_BASE_URL}/v1/learnlists/{learnlistId}/contents`

### Required Scope
This endpoint requires the `learnlists:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "contents": [
        {
            "id": 1180,
            "name": "Intro to Onboarding",
            "shortDescription": "A short welcome video.",
            "type": "Item",
            "displayType": "Item",
            "url": "https://examplecompany.learnamp.com/en/items/intro-to-onboarding",
            "totalTimeEstimate": "< 5 mins",
            "addedBy": {
                "id": 1
            }
        },
        {
            "id": 477,
            "name": "Onboarding Knowledge Check",
            "shortDescription": "Assess what you've learned.",
            "type": "Quiz",
            "displayType": "Quiz",
            "url": "https://examplecompany.learnamp.com/en/quizzes/477",
            "totalTimeEstimate": "< 1 hr",
            "addedBy": {
                "id": 1
            }
        },
        {
            "id": 58,
            "name": "Team Welcome Session",
            "shortDescription": null,
            "type": "Event",
            "displayType": "Event",
            "url": "https://examplecompany.learnamp.com/en/events/58",
            "totalTimeEstimate": "1 hr",
            "addedBy": {
                "id": null
            }
        }
    ]
}

```

`addedBy` contains only the `id` of the user who added the entry. When the entry was added by an external contributor, `addedBy.id` is `null`. For content types that do not track who added them, `addedBy` is omitted entirely.

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Learnlist Users Progress

> View progress of all assigned users through a specified learnlist:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists/123/users_progress' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def users_progress(id, page: 1)
      response = self.class.get("/learnlists/#{id}/users_progress?page=#{page}", { headers: headers })
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

data = Learnamp::Learnlists.new(token).users_progress(123)
```

View progress of all assigned users through a specified learnlist. This is analogous to Learn Amp's Content Log feature, for a single learnlist, and returns the same fields as [Channel Users Progress](#users-progress).

`GET https://{API_BASE_URL}/v1/learnlists/{learnlistId}/users_progress`

### Required Scope
This endpoint requires the `learnlist_users_progress:read` scope.

This end-point will return a paginated array of users who are assigned the specified learnlist. Each element will contain the completion percentage of the learnlist by that user, as well as the datetime (if any) when the learnlist was completed.

Please note: Only assigned users will be returned. Users who are not assigned the specified learnlist will not appear in the array.

When a learnlist is set again as a task with "Require learner to complete again", the learnlist's previous completions no longer count, so this endpoint reports those users as incomplete until they complete it again. Use it rather than Channel Users Progress when you need the current completion of a learnlist that is re-tasked on a cycle.

Also, please note: The completion percentage is a cached value, which is refreshed in the background automatically. The completion percentage shown may therefore take a few minutes to update.

The results are ordered alphabetically by user's last name.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
[
    {
        "userId": 200321,
        "firstName": "John",
        "lastName": "Abc",
        "email": "jabc@test.com",
        "contentName": "Data Protection",
        "contentType": "Learnlist",
        "contentId": 123,
        "completed": false,
        "completionPercent": 50,
        "completedAt": null
    },
    {
        "userId": 200018,
        "firstName": "Hannah",
        "lastName": "Baker",
        "email": "hbaker@test.com",
        "contentName": "Data Protection",
        "contentType": "Learnlist",
        "contentId": 123,
        "completed": true,
        "completionPercent": 100,
        "completedAt": "2026-03-07T15:44:04Z"
    }
]
```

> 404 Not Found - unsuccessful response when learnlist ID does not exist:

```json
{
    "error": "Not found"
}
```
