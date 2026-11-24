# Languages

## Language Description

The languages Learn Amp is available in. Use a language's `value` wherever the API accepts a locale, for example a user's `language`.

## View All Languages

> View all available languages:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/languages' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Languages
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(params = {})
      response = self.class.get('/languages', { query: params, headers: headers })
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

languages = Learnamp::Languages.new(token).all
```

View all languages

`GET https://{API_BASE_URL}/v1/languages`

### Required Scope
This endpoint requires the `languages:read` scope.

Response will be paginated [see pagination](#pagination). Every language fits on the first page at the default page size.

### Response Fields

Field | Description
--------- | -----------
id | Position of the language in the list
value | Locale code, e.g. `en`, `en-US`, `pt-BR`
sample | Same as `value`
label | Name of the language in English

> 200 OK - successful response (first three of the list):

```json
{
    "languages": [
        {
            "id": 0,
            "value": "en",
            "sample": "en",
            "label": "English (UK)"
        },
        {
            "id": 1,
            "value": "en-US",
            "sample": "en-US",
            "label": "English (USA)"
        },
        {
            "id": 2,
            "value": "de",
            "sample": "de",
            "label": "German"
        }
    ]
}
```
