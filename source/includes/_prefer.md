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

`return=minimal` is the only preference we read, and we parse it as RFC 7240 defines it: names and values are case-insensitive, so `Return=Minimal` works; whitespace around the `=` is accepted; the value may be quoted, as `return="minimal"`; and a preference may carry `;parameters`, which we ignore.

`Prefer` may carry several comma-separated preferences. Any others are ignored rather than rejected, so an extra preference will not fail the request — it just has no effect here. That includes `return=representation`, RFC 7240's counterpart to `return=minimal`: we do not act on it, though it describes what you get by default anyway.

Where the same preference appears twice, the first one counts, per RFC 7240. So `Prefer: return=representation, return=minimal` returns the full body.

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
