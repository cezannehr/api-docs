# Users

## User Description

In Learn Amp, a User, is a person who can access the platform. User's belong to your company account, and have a status like Invite Pending, Active or Deactivated.


## View All Users

> View all users in your account:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def all
      response = self.class.get('/users', { headers: headers })
      response.parsed_response if response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

users = Learnamp::Users.new(token).all
```

View all users.

`GET https://{API_BASE_URL}/v1/users`

### Required Scope
This endpoint requires the `users:read` scope.

Response will be paginated [see pagination](#pagination)

### Optional Filters in URL Params

The following URL params may be included, to filter the result set:

`GET https://{API_BASE_URL}/v1/users?filters[email]=test@email.com`


URL Param |Example Value | Description
--------- | ------- | -----------
expanded | true | Optional expanded json. Returns all available user details including custom fields and teams information.
filters[email] | email@test.com | Return users with matching email address
filters[integration_external_id] | EPOS-12345 | Return the user with exactly matching integration external ID
filters[first_name] | John | Return users with matching first name (using `ILIKE '%value%'`)
filters[last_name] | Smith | Return users with matching last name (using `ILIKE '%value%'`)
filters[role] | viewer | Return users with matching role. <br />Possible values: "viewer", "curator", "reporter", "learning_designer", "hr", "admin", "owner"
filters[no_team] | true | Return users that have no team assigned
filters[team_ids] | *[1,2,3]* or *1,2,3* | Return members of any of the given teams by team ID.<br /><br />Param can be array of team ids, or a string of comma seperated team ids
filters[status][] | *[confirmed,invite_pending]* or *confirmed* | Return users matching any of the given statuses. Repeat the param to pass multiple, e.g. `filters[status][]=confirmed&filters[status][]=deactivated`.<br />Param can be an array of statuses, or a string of comma separated statuses.<br />Possible values: "not_invited", "invite_scheduled", "invite_pending", "confirmed", "deactivated"
filters[created_at][from] | "2021-12-31" | Created date range FROM date in ISO 8601 format
filters[created_at][to] | "2022-02-28" | Created date range TO date in ISO 8601 format
filters[updated_at][from] | "2021-12-31" | Updated date range FROM date in ISO 8601 format
filters[updated_at][to] | "2022-02-28" | Updated date range TO date in ISO 8601 format
filters[deactivated_at][from] | "2021-12-31" | Deactivated date range FROM date in ISO 8601 format
filters[deactivated_at][to] | "2022-02-28" | Deactivated date range TO date in ISO 8601 format

> 200 OK - successful response:

```json
{
  "users": [
    {
      "id": 1,
      "firstName": "Test",
      "lastName": "User",
      "jobTitle": "Developer",
      "email": "test@email.com",
      "timeZone": "London",
      "language": "en",
      "role": "viewer",
      "integrationExternalId": "EPOS-12345",
      "hireDate": "2021-01-15",
      "profileUrl": "https://testaccount.learnamp.com/en/users/1",
      "status": {
          "status": "Confirmed",
          "time": "On 29 Nov 16"
      },
    },
  ]
}

```

> 200 OK - successfully response, when param expanded=true is included

```json
{
  "users": [
    {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Developer",
        "email": "test@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "viewer",
        "integrationExternalId": "EPOS-12345",
        "hireDate": "2021-01-15",
        "profileUrl": "https://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        },
        "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
        "manager": {
            "id": 17,
            "firstName": "Test",
            "lastName": "Manager",
            "jobTitle": "Ops Director",
            "email": "test2@email.com",
            "timeZone": "London",
            "language": "en",
            "role": "admin",
            "integrationExternalId": "EPOS-98765",
            "profileUrl": "https://testaccount.learnamp.com/en/users/17",
            "status": {
                "status": "Confirmed",
                "time": "On 20 Feb 17"
            }
        },
        "location": "London, UK",
        "department": "Marketing",
        "primaryTeam": {
            "id": 15,
            "name": "Operations",
            "teamUsersCount": 1,
            "apiTeamPath": "/v1/teams/15.json",
            "apiTeamUsersPath": "/v1/teams/15/users.json",
            "manager": {
              "id": 17,
              "firstName": "Test",
              "lastName": "Manager",
              "jobTitle": "Ops Director",
              "email": "test2@email.com",
              "timeZone": "London",
              "language": "en",
              "role": "admin",
              "profileUrl": "https://testaccount.learnamp.com/en/users/17",
              "status": {
                  "status": "Confirmed",
                  "time": "On 20 Feb 17"
              }
            }
          },
        "secondaryTeams": []
      }
  ]
}
```

## Show a User

> Display details for a single user:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def find(id)
      response = self.class.get("/users/#{id}", { headers: headers })
      response.parsed_response if response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

user = Learnamp::Users.new(token).find(1920)
```

Display user details for one specific user.

`GET https://{API_BASE_URL}/v1/users/{userId}`

### Required Scope
This endpoint requires the `users:read` scope.

> 200 OK - successful response:

```json
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "test@email.com",
    "timeZone": "London",
    "language": "en",
    "role": "viewer",
    "integrationExternalId": "EPOS-12345",
    "occupationRole": "Software Engineer",
    "hireDate": "2021-01-15",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
    "status": {
        "status": "Confirmed",
        "time": "On 29 Nov 16"
    },
    "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
    "manager": {
        "id": 17,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "test2@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "integrationExternalId": "EPOS-98765",
        "profileUrl": "https://testaccount.learnamp.com/en/users/17",
        "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
        }
    },
    "location": "London, UK",
    "department": "Marketing",
    "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 1,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Test",
          "lastName": "Manager",
          "jobTitle": "Ops Director",
          "email": "test2@email.com",
          "timeZone": "London",
          "language": "en",
          "role": "admin",
          "integrationExternalId": "EPOS-98765",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
        }
      }
    },
    "secondaryTeams": []
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Show a User by Integration External ID

> Display details for a single user, looked up by integration external ID:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/users/by_integration_external_id/EPOS-12345' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def find_by_integration_external_id(external_id)
      encoded_id = ERB::Util.url_encode(external_id)
      response = self.class.get("/users/by_integration_external_id/#{encoded_id}", { headers: headers })
      response.parsed_response if response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

user = Learnamp::Users.new(token).find_by_integration_external_id("EPOS-12345")
```

Display user details for the user whose integration external ID exactly matches the given value.

`GET https://{API_BASE_URL}/v1/users/by_integration_external_id/{integrationExternalId}`

The external ID must be URL-encoded when used in the path. For example, an ID of `HR/1001` must be requested as `/v1/users/by_integration_external_id/HR%2F1001`.

Lookups are scoped to your company: an unknown external ID returns `404 Not Found`.

### Required Scope
This endpoint requires the `users:read` scope.

> 200 OK - successful response:

```json
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "test@email.com",
    "timeZone": "London",
    "language": "en",
    "role": "viewer",
    "integrationExternalId": "EPOS-12345",
    "occupationRole": "Software Engineer",
    "hireDate": "2021-01-15",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
    "status": {
        "status": "Confirmed",
        "time": "On 29 Nov 16"
    },
    "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
    "manager": {
        "id": 17,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "test2@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "integrationExternalId": "EPOS-98765",
        "profileUrl": "https://testaccount.learnamp.com/en/users/17",
        "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
        }
    },
    "location": "London, UK",
    "department": "Marketing",
    "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 1,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Test",
          "lastName": "Manager",
          "jobTitle": "Ops Director",
          "email": "test2@email.com",
          "timeZone": "London",
          "language": "en",
          "role": "admin",
          "integrationExternalId": "EPOS-98765",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
        }
    },
    "secondaryTeams": []
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Get Custom Fields

> List the custom fields defined on your account:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/users/custom_fields' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def custom_fields
      response = self.class.get("/users/custom_fields", { headers: headers })
      response.parsed_response if response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

custom_fields = Learnamp::Users.new(token).custom_fields
```

List the custom fields defined on your account, so that you can map your own fields onto them before sending values with [Create a User](#create-a-user) or [Update a user](#update-a-user).

`GET https://{API_BASE_URL}/v1/users/custom_fields`

Custom fields are defined on your account by Learn Amp — they cannot be created, renamed or deleted through the API. This endpoint is read-only, and returns definitions rather than any user's values. A user's own values are returned on the user object as `customFields`.

Each field's `name` is the value to send in the `customFields` array when creating or updating a user.

### Required Scope
This endpoint requires the `users:read` scope.

### Response fields

Field | type | Description
--------- | ---- | -----------
customFieldsEnabled | boolean | Whether custom field values can be written on your account. When `false`, a `customFields` payload sent to the user endpoints is ignored. Definitions are still listed, so this is distinguishable from having none defined.
id | integer | Identifier of the custom field.
name | string | The field's name, as shown in Learn Amp. Send this value in the `customFields` array when creating or updating a user.
namedIdentifier | string or null | An optional stable reference code for the field. Not set on most fields.
validatePresence | boolean | When `true`, the field's value cannot be cleared. See [Clearing custom field values](#clearing-custom-field-values).
validateUniqueness | boolean | When `true`, a value may only be used by one user in your account. Sending a duplicate value returns `400 Bad Request`.
possibleValues | Array(string) or null | When `null`, the field accepts free text. When an array, only those values are accepted and anything else returns `400 Bad Request`.
userEditable | boolean | Whether users can edit their own value for this field.
visibleOnUserProfile | boolean | Whether the field is shown on a user's profile.

> 200 OK - successful response:

```json
{
    "customFieldsEnabled": true,
    "customFields": [
        {
            "id": 109,
            "name": "Contract term",
            "namedIdentifier": null,
            "validatePresence": false,
            "validateUniqueness": false,
            "possibleValues": [
                "Permanent",
                "Fixed term"
            ],
            "userEditable": false,
            "visibleOnUserProfile": true
        },
        {
            "id": 107,
            "name": "Cost Centre",
            "namedIdentifier": null,
            "validatePresence": false,
            "validateUniqueness": false,
            "possibleValues": null,
            "userEditable": false,
            "visibleOnUserProfile": true
        },
        {
            "id": 110,
            "name": "Employee Number",
            "namedIdentifier": "employee_number",
            "validatePresence": true,
            "validateUniqueness": true,
            "possibleValues": null,
            "userEditable": false,
            "visibleOnUserProfile": true
        }
    ]
}
```

## Create a User

> Create a new user in your account:

```shell
curl --location --request POST 'https://{API_BASE_URL}/v1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'email=testuser@test.com' \
--form 'firstName=Test' \
--form 'lastName=User' \
--form 'role=curator' \
--form 'language=fr' \
--form 'jobTitle=Developer' \
--form 'primaryTeamId=15' \
--form 'secondaryTeamIds[]=376' \
--form 'managerId=1' \
--form 'skipInvitation=true' \
--form 'secondaryTeamIds[]=377'
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

    def create(params)
      response = self.class.post("/users", { body: params, headers: headers })
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
  email:  "testuser@test.com",
  firstName: "Test",
  lastName:  "User",
  language: "en",
  jobTitle:  "Developer",
  role: "viewer",
  primaryTeamId: 379,
  secondaryTeamIds:  [376,377],
  managerId: 1,
  skipInvitation:  true,
  hireDate:  "2021-02-28",
  location:  "London",
  department:  "Marketing",
  customFields: [{ name: "Employee ID", value: "123456" }]
}

user = Learnamp::Users.new(token).create(params)

```

Create a user and trigger an invite email.

`POST https://{API_BASE_URL}/v1/users`

### Required Scope
This endpoint requires the `users:create` scope.

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
email | testuser@test.com | Email address of user *(\*)*
firstName | Test | First name of user *(\*)*
lastName | User | Last name of user *(\*)*
integrationExternalId | "EPOS-12345" | Optional third-party identifier for the user. Must be unique within your company.
language | fr | Primary language short code (e.g. `fr`). Must be one of the [supported language codes](#supported-language-codes).
jobTitle | Developer | Job title of user
role | viewer | user's role. One of: viewer, curator, reporter, learning_designer, hr, admin
primaryTeamId | 15 | Team ID of primary team [see Teams](#teams)
primaryTeamIntegrationExternalId | "EPOS-001" | Primary team's `integrationExternalId` [see Teams](#teams). Mutually exclusive with `primaryTeamId`.
secondaryTeamIds | [376,377] | Array of Team IDs of seconary teams
secondaryTeamIntegrationExternalIds | ["EPOS-001","EPOS-002"] | Array of `integrationExternalId`s for secondary teams. Mutually exclusive with `secondaryTeamIds`.
managerId | 1 | User ID of this user's Manager (override manager, not primary team manager). Mutually exclusive with `managerEmail`.
managerEmail | "manager@acme.com" | Email of this user's Manager. Matched case-insensitively against active users in your company. Mutually exclusive with `managerId`.
skipInvitation | true | Skip sending the user an invitation email immediately. If skipInvitation is not set, the user will be immediately sent an invitation email
hireDate | 2021-02-28 | Employment start date for user in ISO 8601 date format
location | London | Primary location of user
department | Marketing | Department of user
customFields | [{ name: "Employee ID", value: "12-34-56" }] | CustomFields param is an array, of name/value pairs for custom fields. Names must match the fields defined on your account — see [Get Custom Fields](#get-custom-fields). See also [Clearing custom field values](#clearing-custom-field-values).

> 201 Created - successful response:

```json
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "test@email.com",
    "timeZone": "London",
    "language": "en",
    "role": "viewer",
    "integrationExternalId": "EPOS-12345",
    "occupationRole": "Software Engineer",
    "hireDate": "2021-01-15",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
    "status": {
        "status": "Confirmed",
        "time": "On 29 Nov 16"
    },
    "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
    "manager": {
        "id": 17,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "test2@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "integrationExternalId": "EPOS-98765",
        "profileUrl": "https://testaccount.learnamp.com/en/users/17",
        "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
        }
    },
    "location": "London, UK",
    "department": "Marketing",
    "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 1,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Test",
          "lastName": "Manager",
          "jobTitle": "Ops Director",
          "email": "test2@email.com",
          "timeZone": "London",
          "language": "en",
          "role": "admin",
          "integrationExternalId": "EPOS-98765",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
        }
      }
    },
    "secondaryTeams": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "email is missing, email is empty, firstName is missing, lastName is missing",
    "fullErrors": {
        "email": [
            "is missing",
            "is empty"
        ],
        "firstName": [
            "is missing"
        ],
        "lastName": [
            "is missing"
        ]
    }
}
```

> 400 Bad request - unknown team `integrationExternalId`:

```json
{
    "error": "primaryTeamIntegrationExternalId must match existing teams integration external IDs",
    "fullErrors": {
        "primaryTeamIntegrationExternalId": [
            "must match existing teams integration external IDs"
        ]
    }
}
```

> 400 Bad request - both id-keyed and external-id-keyed team params sent:

```json
{
    "error": "primaryTeamId, primaryTeamIntegrationExternalId are mutually exclusive"
}
```

> 400 Bad request - unknown `managerEmail`:

```json
{
    "error": "managerEmail must match an existing user email within your company"
}
```

> 400 Bad request - both `managerId` and `managerEmail` sent:

```json
{
    "error": "managerId, managerEmail are mutually exclusive"
}
```

## Update a user

> Update an existing user:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/users/1382' \
--header 'Authorization: Bearer SQn9nZAxgwPJcz-neajmNahBdlc2DRoNbUHwx9A4Rvw' \
--form 'firstName=Jim' \
--form 'lastName=Robinson' \
--form 'jobTitle=Businessman' \
--form 'language=en' \
--form 'primaryTeamId=379' \
--form 'managerId=1' \
--form 'email=jimrobinson@test.com' \
--form 'secondaryTeamIds[]=380'
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

    def update(id, params)
      response = self.class.put("/users/#{id}", { body: params, headers: headers })
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
  email:  "testuser@test.com",
  firstName: "Test2",
  lastName:  "User2",
  language: "en",
  jobTitle:  "Developer",
  role: "viewer",
  primaryTeamId: 379,
  secondaryTeamIds:  [376,377],
  managerId: 1,
  skipInvitation:  true,
  hireDate:  "2021-02-28",
  location:  "London",
  department:  "Marketing",
  occupationRole: "Software Engineer",
  customFields: [{ name: "Employee ID", value: "123456" }]
}

user = Learnamp::Users.new(token).update(1904, params)
```

Update a user's details'.

`PUT https://{API_BASE_URL}/v1/users/{userId}`

### Required Scope
This endpoint requires the `users:update` scope.

### Data in Body

Parameter | type | Example value | Description (* required)
--------- | ---- | ------- | -----------
email | email |testuser@test.com | Email address of user *(\*)*
firstName | string | Test | First name of user *(\*)*
lastName | string | User | Last name of user *(\*)*
integrationExternalId | string | "EPOS-12345" | Optional third-party identifier for the user. Must be unique within your company. Send `null` or `""` to clear an existing value.
language | enum | fr | Primary language short code (e.g. `fr`). Must be one of the [supported language codes](#supported-language-codes).
jobTitle | string |Developer | Job title of user
role | enum | admin | User's system role. One of: viewer, curator, reporter, learning_designer, hr, admin. `owner` and `super_admin` cannot be set via the API. `hr` requires the HR-role package on your company (otherwise returns `400 Bad Request`).
primaryTeamId | integer | 15 | Team ID of primary team [see Teams](#teams)
primaryTeamIntegrationExternalId | string | "EPOS-001" | Primary team's `integrationExternalId` [see Teams](#teams). Mutually exclusive with `primaryTeamId`.
secondaryTeamIds | Array(integer) | [376,377] | Array of Team IDs of seconary teams
secondaryTeamIntegrationExternalIds | Array(string) | ["EPOS-001","EPOS-002"] | Array of `integrationExternalId`s for secondary teams. Mutually exclusive with `secondaryTeamIds`.
managerId | integer | 1 | User ID of this user's Manager (override manager, not primary team manager). Mutually exclusive with `managerEmail`.
managerEmail | string | "manager@acme.com" | Email of this user's Manager. Matched case-insensitively against active users in your company. Mutually exclusive with `managerId`.
hireDate | date | 2021-02-28 | Employment start date for user in ISO 8601 date format
location | string | London | Primary location of user
department | string | Marketing | Department of user
reactivate | boolean | true | Reactivates user if deactivated
occupationRole | string | "Software Engineer" | Name of a skills-matrix role (an "occupation"/role defined in your company) to assign to the user. Assigns it as the user's primary role and applies the role's target skills, removing the need for the user to self-select a role during onboarding. An unknown role name returns `422 Unprocessable Entity`. Returned on the user object as `occupationRole`.
customFields | object | [{ name: "Employee ID", value: "12-34-56" }] | CustomFields param is an array, of name/value pairs for custom fields. See [Clearing custom field values](#clearing-custom-field-values) below.

### Clearing custom field values

Within the `customFields` array each entry is a `{ "name": ..., "value": ... }` pair, matched by `name`:

* To **clear** a custom field, send it with `"value": null` or `"value": ""` — the stored value is removed.
* **Omitting** a field from the `customFields` array leaves that field **unchanged** — omission does **not** clear it.
* A custom field with **presence validation enabled cannot be cleared**: attempting to clear it returns `400 Bad Request` with a validation error and the existing value is retained.
* An unknown custom field `name` returns `404 Not Found`. Use [Get Custom Fields](#get-custom-fields) to check the names defined on your account.
* If your company has custom fields disabled, the entire `customFields` payload is silently ignored and the rest of the update still succeeds.

> 200 OK - successful response:

```json
{
    "id": 1382,
    "firstName": "Jim",
    "lastName": "Robinson",
    "jobTitle": "Businessman",
    "email": "jimrobinson@test.com",
    "timeZone": "London",
    "language": "en",
    "role": "admin",
    "integrationExternalId": "EPOS-13820",
    "occupationRole": "Software Engineer",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1382",
    "status": {
        "status": "Invite pending",
        "time": "Sent 25 Sep 19"
    },
    "avatar": "AVATAR_URL",
    "manager": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Director",
        "email": "test@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "integrationExternalId": "EPOS-12345",
        "profileUrl": "https://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "location": null,
    "primaryTeam": {
        "id": 379,
        "name": "Team 1",
        "teamUsersCount": 3,
        "apiTeamPath": "/v1/teams/379.json",
        "apiTeamUsersPath": "/v1/teams/379/users.json",
        "manager": {
        }
    },
    "secondaryTeams": [
        {
            "id": 380,
            "name": "Team 2 - operations",
            "teamUsersCount": 1,
            "apiTeamPath": "/v1/teams/380.json",
            "apiTeamUsersPath": "/v1/teams/380/users.json",
            "manager": {
            }
        }
    ]
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "email is missing, email is empty, firstName is missing, lastName is missing",
    "fullErrors": {
        "email": [
            "is missing",
            "is empty"
        ],
        "firstName": [
            "is missing"
        ],
        "lastName": [
            "is missing"
        ]
    }
}
```

> 400 Bad request - unknown `managerEmail`:

```json
{
    "error": "managerEmail must match an existing user email within your company"
}
```

> 400 Bad request - both `managerId` and `managerEmail` sent:

```json
{
    "error": "managerId, managerEmail are mutually exclusive"
}
```

> 400 Bad request - duplicate `integrationExternalId` within your company:

```json
{
    "error": "Validation failed: Integration external id has already been taken",
    "fullErrors": {
        "integrationExternalId": [
            "has already been taken"
        ]
    }
}
```

> 400 Bad request - clearing a custom field that has presence validation enabled (the existing value is retained):

```json
{
    "error": "Validation failed: Value can't be blank",
    "fullErrors": {
        "value": [
            "can't be blank"
        ]
    }
}
```

> 404 Not Found - unknown custom field `name`:

```json
{
    "error": "Not found"
}
```

> 422 Unprocessable Entity - unknown `occupationRole` name:

```json
{
    "error": "occupationRole does not match an existing role in your company"
}
```

A user may also be updated by `integrationExternalId` via a dedicated subroute — see [Update a User by Integration External ID](#update-a-user-by-integration-external-id).

## Update a User by Integration External ID

> Update an existing user, looked up by `integrationExternalId`:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/users/by_integration_external_id/EPOS-12345' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'jobTitle=Senior Developer' \
--form 'location=Edinburgh'
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

    def update_by_integration_external_id(integration_external_id, params)
      response = self.class.put(
        "/users/by_integration_external_id/#{integration_external_id}",
        { body: params, headers: headers }
      )
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
  jobTitle: "Senior Developer",
  location: "Edinburgh"
}

user = Learnamp::Users.new(token).update_by_integration_external_id("EPOS-12345", params)
```

Update a user without knowing their Learn Amp `id`. The user is matched by `integrationExternalId` within your company.

`PUT https://{API_BASE_URL}/v1/users/by_integration_external_id/{integrationExternalId}`

### Required Scope
This endpoint requires the `users:update` scope.

### Data in Body

Accepts the same body parameters as [Update a User](#update-a-user), including `integrationExternalId` itself — sending a new value renames the user's external identifier, sending `null` or `""` clears it.

### Behaviour

* Lookup is scoped to your company — an unknown `integrationExternalId` returns `404 Not Found`.
* Renaming to an `integrationExternalId` already used by another user in your company returns `400 Bad Request`.
* Purely numeric values (e.g. `"42"`) are reachable through this subroute regardless of any user's `id`.
* `customFields` clearing and `occupationRole` behave exactly as on [Update a User](#update-a-user) — see [Clearing custom field values](#clearing-custom-field-values).

> 200 OK - successful response:

```json
{
    "id": 1382,
    "firstName": "Jim",
    "lastName": "Robinson",
    "jobTitle": "Senior Developer",
    "email": "jimrobinson@test.com",
    "timeZone": "London",
    "language": "en",
    "role": "admin",
    "integrationExternalId": "EPOS-12345",
    "occupationRole": "Software Engineer",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1382",
    "status": {
        "status": "Confirmed",
        "time": "On 25 Sep 19"
    },
    "location": "Edinburgh",
    "primaryTeam": null,
    "secondaryTeams": []
}
```

> 404 Not Found - no user matches the supplied `integrationExternalId`:

```json
{
    "error": "Not found"
}
```

> 400 Bad request - rename to a value already used by another user in your company:

```json
{
    "error": "Validation failed: Integration external id has already been taken",
    "fullErrors": {
        "integrationExternalId": [
            "has already been taken"
        ]
    }
}
```

## Deactivate a User

> Deactivate a user:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/users/1/deactivate' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def deactivate(id)
      response = self.class.put("/users/#{id}/deactivate", { headers: headers })
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

Learnamp::Users.new(token).deactivate(1904)
```

Deactivate a user, so that they can no longer login. Their data is not removed.

`PUT https://{API_BASE_URL}/v1/users/{userId}/deactivate`

### Required Scope
This endpoint requires the `users:deactivate` scope.

> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Reactivate a User

> Reactivate a deactivated user:

```shell
curl --location --request PUT 'https://{API_BASE_URL}/v1/users/1/reactivate' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def reactivate(id)
      response = self.class.put("/users/#{id}/reactivate", { headers: headers })
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

Learnamp::Users.new(token).reactivate(1904)
```

Reactivate a user, so that they login again.

`PUT https://{API_BASE_URL}/v1/users/{userId}/reactivate`

### Required Scope
This endpoint requires the `users:reactivate` scope.

> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Delete a User

> Delete a user:

```shell
curl --location --request DELETE 'https://{API_BASE_URL}/v1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def delete(id)
      response = self.class.delete("/users/#{id}", { headers: headers })
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

Learnamp::Users.new(token).delete(1904)
```

Delete a user, so that they can no longer login. Their data is removed.

`DELETE https://{API_BASE_URL}/v1/users/{userId}`

### Required Scope
This endpoint requires the `users:delete` scope.

> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Channels Progress

> View progress of all assigned channels by the specified user:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/users/567/channels_progress' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
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

    def channels_progress(user_id)
      response = self.class.get("/channels/#{user_id}/users_progress", { headers: headers })
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

data = Learnamp::Users.new(token).Channels_progress(123)
```

View progress of all assigned channels by a specified user. This is analogous to Learn Amp's People Log feature, for a single user.

`GET https://{API_BASE_URL}/v1/users/{userId}/channels_progress`

### Required Scope
This endpoint requires the `user_channels_progress:read` scope.

This end-point will return a paginated array of channels that are assigned to the specified user. Each element will contain the completion percentage of the channel by that user, as well as the datetime (if any) when the channel was completed.

Please note: The completion percentage is a cached value, which is refreshed in the background automatically. The completion percentage shown may therefore take a few minutes to update.

The results are ordered alphabetically by created at date of the channel, most recent first.

The end-point will accept a `userId` of a deactivated user, and return the deactivated users's channel progress.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
[
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Favourites",
        "content_type": "Channel",
        "content_id": 1,
        "completed": false,
        "completion_percent": 50,
        "completed_at": null
    },
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Compliance Training",
        "content_type": "Channel",
        "content_id": 2,
        "completed": true,
        "completion_percent": 100,
        "completed_at": "2023-04-04T15:44:04Z"
    },
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Business Essentials",
        "content_type": "Channel",
        "content_id": 3,
        "completed": false,
        "completion_percent": 0,
        "completed_at": null
    }
]

```

> 404 Not Found - unsuccessful response when user ID does not exist:

```json
{
    "error": "Not found"
}
```

## Supported Language Codes

The `language` field on the [Create](#create-a-user) and [Update](#update-a-user) user endpoints accepts one of the codes below.

<aside class="notice">Send the <strong>code</strong> (the value in the left column, e.g. <code>fr</code>) — not the language name. The Language column is for reference only.</aside>

Code | Language
---- | --------
`en` | UK English
`en-US` | US English
`de` | German
`es` | Spanish
`es-CO` | Colombian Spanish
`fr` | French
`fr-CA` | Canadian French
`it` | Italian
`nl` | Dutch
`pt-BR` | Brazilian Portuguese
`pl` | Polish
`ru` | Russian
`no` | Norwegian
`sv` | Swedish
`da` | Danish
`zh-CN` | Simplified Chinese
`zh-TW` | Traditional Chinese
`ja` | Japanese
`ar` | Arabic
`sk` | Slovak
`ca` | Catalan
`cy` | Welsh
`el` | Greek
`hu` | Hungarian
`pt-PT` | European Portuguese
`ro` | Romanian
`tr` | Turkish
