# Roles

## Role Description

Every user has one system role, which decides what they can do in Learn Amp. Use a role's `value` wherever the API accepts a role, for example a user's `role`.

There are two lists, because who can assign a role depends on the role of the person assigning it:

* **View All Roles** returns the roles an admin can assign.
* **View All Owner Roles** returns the roles an owner can assign. That is the same list, plus `owner`.

The `hr` role appears in both lists only when the company has the HR role enabled.

Labels are in the company's default language.

## View All Roles

> View all roles an admin can assign:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/roles' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Roles
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(params = {})
      response = self.class.get('/roles', { query: params, headers: headers })
      response.parsed_response
    end

    def owner(params = {})
      response = self.class.get('/roles/owner', { query: params, headers: headers })
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

roles = Learnamp::Roles.new(token).all
```

View all roles an admin can assign

`GET https://{API_BASE_URL}/v1/roles`

### Required Scope
This endpoint requires the `roles:read` scope.

Response will be paginated [see pagination](#pagination). Every role fits on the first page at the default page size.

### Response Fields

Field | Description
--------- | -----------
id | Role identifier
value | Role code, e.g. `viewer`, `admin`
sample | Same as `value`
label | Display name of the role

> 200 OK - successful response (company with the HR role enabled):

```json
{
    "roles": [
        {
            "id": 0,
            "value": "viewer",
            "sample": "viewer",
            "label": "Learner"
        },
        {
            "id": 1,
            "value": "curator",
            "sample": "curator",
            "label": "Curator"
        },
        {
            "id": 4,
            "value": "reporter",
            "sample": "reporter",
            "label": "Reporter"
        },
        {
            "id": 6,
            "value": "learning_designer",
            "sample": "learning_designer",
            "label": "Learning Designer"
        },
        {
            "id": 5,
            "value": "hr",
            "sample": "hr",
            "label": "HR"
        },
        {
            "id": 2,
            "value": "admin",
            "sample": "admin",
            "label": "Admin"
        }
    ]
}
```

## View All Owner Roles

> View all roles an owner can assign:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/roles/owner' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
owner_roles = Learnamp::Roles.new(token).owner
```

View all roles an owner can assign

`GET https://{API_BASE_URL}/v1/roles/owner`

### Required Scope
This endpoint requires the `roles:read` scope.

Response will be paginated [see pagination](#pagination). The fields are the same as in [View All Roles](#view-all-roles).

> 200 OK - successful response (company with the HR role enabled):

```json
{
    "roles": [
        {
            "id": 0,
            "value": "viewer",
            "sample": "viewer",
            "label": "Learner"
        },
        {
            "id": 1,
            "value": "curator",
            "sample": "curator",
            "label": "Curator"
        },
        {
            "id": 4,
            "value": "reporter",
            "sample": "reporter",
            "label": "Reporter"
        },
        {
            "id": 6,
            "value": "learning_designer",
            "sample": "learning_designer",
            "label": "Learning Designer"
        },
        {
            "id": 5,
            "value": "hr",
            "sample": "hr",
            "label": "HR"
        },
        {
            "id": 2,
            "value": "admin",
            "sample": "admin",
            "label": "Admin"
        },
        {
            "id": 3,
            "value": "owner",
            "sample": "owner",
            "label": "Owner"
        }
    ]
}
```
