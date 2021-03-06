# Elvanto API Ruby Library

This library is all set to go with version 1 of the <a href="https://www.ElvantoAPI.com/api/" target="_blank">Elvanto API</a>.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'elvanto-api'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install elvanto-api

## Authenticating

The Elvanto API supports authentication using either <a href="https://www.ElvantoAPI.com/api/getting-started/#oauth" target="_blank">OAuth 2</a> or an <a href="https://www.ElvantoAPI.com/api/getting-started/#api_key" target="_blank">API key</a>.

### What is This For?

* Quick summary
This is an API wrapper to use in conjunction with an Elvanto account. This wrapper can be used by developers to develop programs for their own churches, or to design integrations to share to other churches using OAuth authentication.
* Version 1.0

### Using OAuth 2

This library provides functionality to help you obtain an Access Token and Refresh token. The first thing your application should do is redirect your user to the Elvanto authorization URL where they will have the opportunity to approve your application to access their Elvanto account. You can get this authorization URL by using the `authorize_url` method, like so:

```ruby
authorize_url = ElvantoAPI.authorize_url(client_id, redirect_uri, scope, state)
// Redirect your users to authorize_url.
```

If your user approves your application, they will then be redirected to the `redirect_uri` you specified, which will include a `code` parameter, and optionally a `state` parameter in the query string. Your application should implement a handler which can exchange the code passed to it for an access token, using `exchange_token` like so:

```ruby
response = ElvantoAPI.exchange_token(client_id, client_secret, code, redirect_uri)

access_token = response[:access_token]
expires_in = response[:expires_in]
refresh_token = response[:refresh_token]
// Save access_token, expires_in and refresh_token.
```

At this point you have an access token and refresh token for your user which you should store somewhere convenient so that your application can look up these values when your user wants to make future Elvanto API calls.

Once you have an access token and refresh token for your user, you can authenticate and make further API calls like so:

```ruby
ElvantoAPI.configure({:access_token=>access_token})
all_people = ElvantoAPI.call("people/getAll")
```

All OAuth tokens have an expiry time, and can be renewed with a corresponding refresh token. If your access token expires when attempting to make an API call, you will receive an error response, so your code should handle this. Here's an example of how you could do this:

```ruby
result = ElvantoAPI.refresh_token(refresh_token)
```

### Using an API key

```ruby
ElvantoAPI.configure({:api_key=>api_key})
people = ElvantoAPI.call("people/getAll")
```

## Documentation

Documentation can be found on the <a href="https://www.elvanto.com/api/" target="_blank">Elvanto API website</a>.

## Updates

Follow our <a href="http://twitter.com/ElvantoAPI" target="_blank">Twitter</a> to keep up-to-date with changes in the API.

## Support

For bugs with the API Ruby Wrapper please use the <a href="https://github.com/elvanto/elvanto-ruby/issues">Issue Tracker</a>.

For suggestions on the API itself, please <a href="http://support.ElvantoAPI.com/support/discussions/forums/1000123316" target="_blank">post in the forum</a> or contact us <a href="http://support.ElvantoAPI.com/support/tickets/new/" target="_blank">via our website</a>.
