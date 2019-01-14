# OmniAuth::OpenIDConnect

Originally was [omniauth-openid-connect](https://github.com/jjbohn/omniauth-openid-connect)

I've forked this repository and launch as separate gem because maintaining of original was dropped.

[![Build Status](https://travis-ci.org/m0n9oose/omniauth_openid_connect.png?branch=master)](https://travis-ci.org/m0n9oose/omniauth_openid_connect)

## Installation

Add this line to your application's Gemfile:

    gem 'omniauth_openid_connect'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install omniauth_openid_connect

## Usage

Example Omniauth configuration

```ruby
config.omniauth :openid_connect, {
  name:          :my_provider,
  scope:         %i[openid email profile address],
  response_type: :code,
  client_options: {
    port:         443,
    scheme:       "https",
    host:         "myprovider.com",
    identifier:   ENV["OP_CLIENT_ID"],
    secret:       ENV["OP_SECRET_KEY"],
    redirect_uri: "http://myapp.com/users/auth/openid_connect/callback",
  },
}
```

Example Devise configuration

```ruby
# In config/initializers/devise.rb

config.omniauth :openid_connect, {
  strategy_class: OmniAuth::Strategies::OpenIDConnect,
  name:           :'openid_connect',
  scope:          [:openid, :user, :email, :profile, :address],
  issuer:         'http://localhost:8080/my/issuer',
  discovery:      true, # Only works via HTTPS
  client_options: { # More options can be found by inspecting the openid_connect gem here https://rubygems.org/gems/openid_connect
    redirect_uri:           "http://localhost:3000/users/auth/openid_connect/callback",
    port:                   80,
    scheme:                 "http",
    host:                   "localhost",
    authorization_endpoint: '/auth/openid-connect/auth',
    token_endpoint:         '/auth/openid-connect/token',
    userinfo_endpoint:      '/auth/openid-connect/userinfo',
    jwks_uri:               '/auth/protocol/openid-connect/certs',
    identifier:             'my-client-identifier',
    secret:                 "my-client-secret",
  },
}
```

Configuration details:
  * `name` is arbitrary, I recommend using the name of your provider. The name
  configuration exists because you could be using multiple OpenID Connect
  providers in a single app.
  * Although `response_type` is an available option, currently, only `:code`
  is valid. There are plans to bring in implicit flow and hybrid flow at some
  point, but it hasn't come up yet for me. Those flows aren't best practive for
  server side web apps anyway and are designed more for native/mobile apps.
  * If you want to pass `state` paramete by yourself. You can set Proc Object.
  e.g. `state: Proc.new{ SecureRandom.hex(32) }`
  * `nonce` is optional. If don't want to pass "nonce" parameter to provider, You should specify
  `false` to `send_nonce` option. (default true)
  * Support for other client authentication methods. If don't specified
  `:client_auth_method` option, automatically set `:basic`.
  * Use "OpenID Connect Discovery", You should specify `true` to `discovery` option. (default false)
  * In "OpenID Connect Discovery", generally provider should have Webfinger endpoint.
  If provider does not have Webfinger endpoint, You can specify "Issuer" to option.
  e.g. `issuer: "https://myprovider.com"`
  It means to get configuration from "https://myprovider.com/.well-known/openid-configuration".
  * `skip_verification`: [`true` | `false`]. Use this if you are having trouble getting your JWK and JWS verified. There is probably something wrong with your config if you need to use this, and it is recommended that you use this option as a debug mechanism during development only.


For the full low down on OpenID Connect, please check out
[the spec](http://openid.net/specs/openid-connect-core-1_0.html).

## Contributing

1. Fork it ( http://github.com/m0n9oose/omniauth-openid-connect/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
