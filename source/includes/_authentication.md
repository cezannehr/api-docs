# Authentication

> Generate an Access Token from the Auth Token URL:

```shell
curl --location --request POST 'https://{API_BASE_URL}/oauth/token' \
--form 'client_id=YOUR-CLIENT-ID' \
--form 'client_secret=YOUR-CLIENT-SECRET' \
--form 'grant_type=client_credentials'
```

```ruby
module Learnamp
  class Auth
    include HTTParty
    base_uri ENV['BASE_URL']

    def self.access_token
      post('/oauth/token',
           body: {
             grant_type: 'client_credentials',
             client_id: ENV['CLIENT_ID'],
             client_secret: ENV['CLIENT_SECRET']
           })
    end
  end
end
-
response = Learnamp::Auth.access_token
puts response["access_token"] if response.ok?
```
> 200 OK - successful response:

```json
{
  "access_token":"abcd1234efgh5678",
  "token_type":"Bearer",
  "expires_in":7200,
  "scope":"public",
  "created_at":1601480215
}
```

The Learn Amp API is secured using the OAuth2 Client Credentials flow.

Before calls can be made to the API, an **access token** must be retrieved from the Auth Token URL:

`POST https://{API_BASE_URL}/oauth/token`


### Data in Body

Parameter | Value | Description
--------- | ------- | -----------
client_id | YOUR-CLIENT-ID | API Credentials Client ID
client_secret | YOUR-CLIENT-SECRET | API Credentials Client Secret
grant_type | client_credentials | Required oath2 grant type

<aside class="notice">
You must replace <code>YOUR-CLIENT-ID</code> and <code>YOUR-CLIENT-SECRET</code> with your API credentials.
</aside>

Subsequent API calls then require this access token to be present in an **Authorization header**:

`Authorization: Bearer YOUR-ACCESS-TOKEN`

<aside class="success">
A successful response will return an <code>access_token</code>, please replace <code>YOUR-ACCESS-TOKEN</code> with this value in all authorization headers.
</aside>

## OAuth Scopes

The Learn Amp API uses OAuth 2.0 scopes to control access to different parts of the API. When requesting an access token, you can specify which scopes you need access to.

By default, tokens are issued with the `public` scope, which provides basic access. For more specific operations, you should request additional scopes.

Legacy scopes are created with full access, and implicitly request and are granted the `public` scope. Moving forward, API credentials should be provisioned with the specific scopes they require.

<aside class="warning">
If your API credentials were provisioned with specific scopes only (no <code>public</code>), you <strong>must</strong> pass an explicit <code>scope</code> parameter in your token request. Omitting <code>scope</code> falls back to the default <code>public</code> scope, which your credentials do not allow, and the request will fail with <code>invalid_scope</code>.
</aside>

### Available Scopes

Scope | Description
--------- | -----------
public | Basic access scope - included by default
items:read | Read access to items
items:create | Create new items
items:update | Update existing items
items:delete | Delete items
items:complete | Mark items as complete
activities:read | Read access to activities
enrollments:read | Read access to enrollments
enrollments:create | Create new enrollments
events:read | Read access to events
teams:read | Read access to teams
teams:create | Create new teams
teams:update | Update existing teams
teams:delete | Delete teams
team_users:read | Read access to team users
team_users:create | Add users to teams
team_users:delete | Remove users from teams
users:read | Read access to users
users:create | Create new users
users:update | Update existing users
users:delete | Delete users
users:deactivate | Deactivate users
users:reactivate | Reactivate users
communities_posts:read | Read access to community posts
tasks:read | Read access to tasks
verbs:read | Read access to verbs
languages:read | Read access to languages
learnlists:read | Read access to learnlists
channels:read | Read access to channels
channel_users_progress:read | Read access to channel users progress
learnlist_users_progress:read | Read access to learnlist users progress
user_channels_progress:read | Read access to user channels progress
roles:read | Read access to roles

### Requesting Scopes

To request specific scopes, add the `scope` parameter to your token request:

```shell
curl --location --request POST 'https://{API_BASE_URL}/oauth/token' \
--form 'client_id=YOUR-CLIENT-ID' \
--form 'client_secret=YOUR-CLIENT-SECRET' \
--form 'grant_type=client_credentials' \
--form 'scope=items:read items:create'
```

```ruby
module Learnamp
  class Auth
    include HTTParty
    base_uri ENV['BASE_URL']

    def self.access_token
      post('/oauth/token',
           body: {
             grant_type: 'client_credentials',
             client_id: ENV['CLIENT_ID'],
             client_secret: ENV['CLIENT_SECRET'],
             scope: 'items:read items:create'
           })
    end
  end
end

response = Learnamp::Auth.access_token
puts response["access_token"] if response.ok?
```
