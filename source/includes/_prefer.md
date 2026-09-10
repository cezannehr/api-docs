# Minimal Responses

By default, the update endpoints return the full updated record in the response body. If your integration does not read that body — for example, because it verifies the write with a separate `GET`, or simply checks the status code — you can ask us not to build it.

Send the [RFC 7240](https://www.rfc-editor.org/rfc/rfc7240) `Prefer` header:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/users/1904' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Prefer: return=minimal' \
--form 'jobTitle=Developer'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    # Returns 204 with no body -- check the status, not the response.
    def update_minimal(id, params)
      self.class.put("/users/#{id}", { body: params, headers: headers })
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}",
        'Prefer' => 'return=minimal'
      }
    end
  end
end
```

> 204 No Content - the update succeeded and no body was rendered:

```json

```

The update is applied exactly as normal. Only the response changes.

## Request

Request Header | Example value | Description
--------- | ------- | -----------
Prefer | return=minimal | Apply the update, then respond without rendering the record

The header is matched case-insensitively, and may carry other comma-separated preferences alongside it (`Prefer: return=minimal, wait=100`). Any preference we do not recognise is ignored.

Omit the header and the response is unchanged, so adding it is safe to roll out one integration at a time.

## Response

With the header applied, you get `204 No Content`, an empty body, and confirmation that we honoured the preference:

Response Header | Example value | Description
--------- | ------- | -----------
Preference-Applied | return=minimal | We skipped rendering the record

<aside class="notice">Check for <code>Preference-Applied</code> rather than assuming the preference took effect. On any endpoint that does not support it, the header is ignored and you get the usual <code>200</code> and full body — so a client that treats every response as empty would silently discard data.</aside>

## Supported endpoints

`Prefer: return=minimal` is currently honoured on the **update** endpoints only:

Method | Endpoint
------ | --------
PUT | [`/v1/users/{userId}`](#update-a-user)
PUT | [`/v1/users/by_integration_external_id/{integrationExternalId}`](#update-a-user-by-integration-external-id)
PUT | [`/v1/teams/{teamId}`](#update-a-team)
PUT | [`/v1/items/{itemId}`](#update-an-item)

<aside class="warning">Create (<code>POST</code>) endpoints do <strong>not</strong> support it yet, and will return <code>201</code> with the full body as usual. If you set <code>Prefer</code> globally in your HTTP client configuration, expect it to apply to updates and be ignored on creates.</aside>

Endpoints that already return `204 No Content` — deactivate, reactivate, and the `DELETE` endpoints — need nothing, as they render no body in the first place.
