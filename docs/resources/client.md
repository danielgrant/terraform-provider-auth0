---
layout: "auth0"
page_title: "Auth0: auth0_client"
description: |-
  With this resource, you can create and configure applications that use Auth0 for authentication.
---

# auth0_client

With this resource, you can set up applications that use Auth0 for authentication and configure allowed callback URLs and secrets for these applications. Depending on your plan, you may also configure add-ons to allow your application to call another application's API (such as Firebase and AWS) on behalf of an authenticated user.

## Example Usage

```hcl
resource "auth0_client" "my_client" {
  name = "Application - Acceptance Test"
  description = "Test Applications Long Description"
  app_type = "non_interactive"
  custom_login_page_on = true
  is_first_party = true
  is_token_endpoint_ip_header_trusted = true
  token_endpoint_auth_method = "client_secret_post"
  oidc_conformant = false
  initiate_login_uri = "https://example.com/login"
  callbacks = [ "https://example.com/callback" ]
  allowed_origins = [ "https://example.com" ]
  allowed_clients = [ "https://allowed.example.com" ]
  grant_types = [ "authorization_code", "http://auth0.com/oauth/grant-type/password-realm", "implicit", "password", "refresh_token" ]
  organization_usage = "deny"
  organization_require_behavior = "no_prompt"
  allowed_logout_urls = [ "https://example.com" ]
  web_origins = [ "https://example.com" ]
  jwt_configuration {
    lifetime_in_seconds = 300
    secret_encoded = true
    alg = "RS256"
    scopes = {
      foo = "bar"
    }
  }
  refresh_token {
    rotation_type = "rotating"
    expiration_type = "expiring"
    leeway = 15
    token_lifetime = 84600
    infinite_idle_token_lifetime = true
    infinite_token_lifetime      = false
    idle_token_lifetime          = 1296000
  }
  client_metadata = {
    foo = "zoo"
  }
  addons {
    firebase = {
      client_email = "john.doe@example.com"
      lifetime_in_seconds = 1
      private_key = "wer"
      private_key_id = "qwreerwerwe"
    }
    samlp {
      audience = "https://example.com/saml"
      mappings = {
        email = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
        name = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
      }
      create_upn_claim = false
      passthrough_claims_with_no_mapping = false
      map_unknown_claims_as_is = false
      map_identities = false
      name_identifier_format = "urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
      name_identifier_probes = [
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
      ]
      signing_cert = "pemcertificate"
    }
  }
  mobile {
    ios {
      team_id = "9JA89QQLNQ"
      app_bundle_identifier = "com.my.bundle.id"
    }
  }
}
```

## Argument Reference

Arguments accepted by this resource include:

* `name` - (Required) String. Name of the client.
* `description` - (Optional) String, (Max length = 140 characters). Description of the purpose of the client.
* `client_secret_rotation_trigger` - (Optional) Map.
* `app_type` - (Optional) String. Type of application the client represents. Options include `native`, `spa`, `regular_web`, `non_interactive`, `rms`, `box`, `cloudbees`, `concur`, `dropbox`, `mscrm`, `echosign`, `egnyte`, `newrelic`, `office365`, `salesforce`, `sentry`, `sharepoint`, `slack`, `springcm`, `zendesk`, `zoom`.
* `logo_uri` - (Optional) String. URL of the logo for the client. Recommended size is 150px x 150px. If none is set, the default badge for the application type will be shown.
* `is_first_party` - (Optional) Boolean. Indicates whether or not this client is a first-party client.
* `is_token_endpoint_ip_header_trusted` - (Optional) Boolean. Indicates whether or not the token endpoint IP header is trusted.
* `oidc_conformant` - (Optional) Boolean. Indicates whether or not this client will conform to strict OIDC specifications.
* `initiate_login_uri` - (Optional) String. Initiate login uri, must be https.
* `callbacks` - (Optional) List(String). URLs that Auth0 may call back to after a user authenticates for the client. Make sure to specify the protocol (https://) otherwise the callback may fail in some cases. With the exception of custom URI schemes for native clients, all callbacks should use protocol https://.
* `allowed_logout_urls` - (Optional) List(String). URLs that Auth0 may redirect to after logout.
* `grant_types` - (Optional) List(String). Types of grants that this client is authorized to use.
* `organization_usage` - (Optional) String. Defines how to proceed during an authentication transaction with regards an organization. Can be `deny` (default), `allow` or `require`.
* `organization_require_behavior` - (Optional) String. Defines how to proceed during an authentication transaction when `organization_usage = "require"`. Can be `no_prompt` (default) or `pre_login_prompt`.
* `allowed_origins` - (Optional) List(String). URLs that represent valid origins for cross-origin resource sharing. By default, all your callback URLs will be allowed.
* `allowed_clients` - (Optional) List(String). List of applications ID's that will be allowed to make delegation request. By default, all applications will be allowed.
* `web_origins` - (Optional) List(String). URLs that represent valid web origins for use with web message response mode.
* `jwt_configuration` - (Optional) List(Resource). Configuration settings for the JWTs issued for this client. For details, see [JWT Configuration](#jwt-configuration).
* `refresh_token` - (Optional) List(Resource). Configuration settings for the refresh tokens issued for this client.  For details, see [Refresh Token Configuration](#refresh-token-configuration).
* `encryption_key` - (Optional) Map(String).
* `sso` - (Optional) Boolean. Indicates whether or not the client should use Auth0 rather than the IdP to perform Single Sign-On (SSO). True = Use Auth0.
* `sso_disabled` - (Optional) Boolean. Indicates whether or not SSO is disabled.
* `cross_origin_auth` - (Optional) Boolean. Indicates whether or not the client can be used to make cross-origin authentication requests.
* `cross_origin_loc` - (Optional) String. URL for the location on your site where the cross-origin verification takes place for the cross-origin auth flow. Used when performing auth in your own domain instead of through the Auth0-hosted login page.
* `custom_login_page_on` - (Optional) Boolean. Indicates whether or not a custom login page is to be used.
* `custom_login_page` - (Optional) String. Content of the custom login page.
* `form_template` - (Optional) String. Form template for WS-Federation protocol.
* `addons` - (Optional) List(Resource). Configuration settings for add-ons for this client. For details, see [Add-ons](#add-ons).
* `token_endpoint_auth_method` - (Optional) String. Defines the requested authentication method for the token endpoint. Options include `none` (public client without a client secret), `client_secret_post` (client uses HTTP POST parameters), `client_secret_basic` (client uses HTTP Basic).
* `client_metadata` - (Optional) Map(String)
* `mobile` - (Optional) List(Resource). Configuration settings for mobile native applications. For details, see [Mobile](#mobile).
* `native_social_login` - (Optional) List(Resource). Configuration settings to toggle native social login for mobile native applications. For details, see [Native Social Login](#native-social-login)

### JWT Configuration

`jwt_configuration` supports the following arguments:

* `lifetime_in_seconds` - (Optional) Integer. Number of seconds during which the JWT will be valid.
* `secret_encoded` - (Optional) Boolean. Indicates whether or not the client secret is base64 encoded.
* `scopes` - (Optional) Map(String). Permissions (scopes) included in JWTs.
* `alg` - (Optional) String. Algorithm used to sign JWTs.

### Refresh Token Configuration

`refresh_token` configuration supports the following arguments:

* `rotation_type` - (Optional) String. Options include `rotating`, `non-rotating`. When `rotating`, exchanging a refresh token will cause a new refresh token to be issued and the existing token will be invalidated. This allows for automatic detection of token reuse if the token is leaked.
* `leeway` - (Optional) Integer. The amount of time in seconds in which a refresh token may be reused without trigging reuse detection.
* `expiration_type` - (Optional unless `rotation_type` is `rotating`) String. Options include `expiring`, `non-expiring`. Whether a refresh token will expire based on an absolute lifetime, after which the token can no longer be used. If rotation is `rotating`, this must be set to `expiring`.
* `token_lifetime` - (Optional) Integer. The absolute lifetime of a refresh token in seconds.
* `infinite_idle_token_lifetime` - (Optional) Boolean, (Default=false) Whether or not inactive refresh tokens should be remain valid indefinitely.
* `infinite_token_lifetime` - (Optional) Boolean, (Default=false) Whether or not refresh tokens should remain valid indefinitely. If false, `token_lifetime` should also be set
* `idle_token_lifetime` - (Optional) Integer. The time in seconds after which inactive refresh tokens will expire.

### Add-ons

`addons` supports the following arguments:

* `aws` - (Optional) String
* `azure_blob` - (Optional) String
* `azure_sb` - (Optional) String
* `box` - (Optional) String
* `cloudbees` - (Optional) String
* `concur` - (Optional) String
* `dropbox`- (Optional) String
* `echosign`- (Optional) String
* `egnyte`- (Optional) String
* `firebase`- (Optional) String
* `mscrm` - (Optional) String
* `newrelic`- (Optional) String
* `office365`- (Optional) String
* `rms` - (Optional) String
* `salesforce`- (Optional) String
* `salesforce_api`- (Optional) String
* `salesforce_sandbox_api`- (Optional) String
* `samlp` - (Optional) List(Resource). Configuration settings for a SAML add-on. For details, see [SAML](#saml).
* `layer`- (Optional) String
* `sap_api`- (Optional) String
* `sentry` - (Optional) String
* `sharepoint`- (Optional) String
* `slack` - (Optional) String
* `springcm`- (Optional) String
* `wams`- (Optional) String
* `wsfed`- (Optional) String
* `zendesk`- (Optional) String
* `zoom`- (Optional) String

### SAML

`samlp` supports the following arguments:

* `audience` - (Optional) String. Audience of the SAML Assertion. Default will be the Issuer on SAMLRequest.
* `recipient` - (Optional) String. Recipient of the SAML Assertion (SubjectConfirmationData). Default is AssertionConsumerUrl on SAMLRequest or Callback URL if no SAMLRequest was sent.
* `create_upn_claim` - (Optional) Boolean, (Default=true) Indicates whether or not a UPN claim should be created.
* `passthrough_claims_with_no_mapping` - (Optional) Boolean, (Default=true). Indicates whether or not to passthrough claims that are not mapped to the common profile in the output assertion.
* `map_unknown_claims_as_is` - (Optional) Boolean, (Default=false). Indicates whether or not to add a prefix of `http://schema.auth0.com` to any claims that are not mapped to the common profile when passed through in the output assertion.
* `map_identities` - (Optional) Boolean, (Default=true). Indicates whether or not to add additional identity information in the token, such as the provider used and the access_token, if available.
* `signature_algorithm` - (Optional) String, (Default=`rsa-sha1`). Algorithm used to sign the SAML Assertion or response. Options include `rsa-sha1` and `rsa-sha256`.
* `digest_algorithm` - (Optional) String, (Default=`sha1`). Algorithm used to calculate the digest of the SAML Assertion or response. Options include `defaultsha1` and `sha256`.
* `destination` - (Optional) String. Destination of the SAML Response. If not specified, it will be AssertionConsumerUrlof SAMLRequest or Callback URL if there was no SAMLRequest.
* `lifetime_in_seconds` - (Optional) Integer, (Default=3600). Number of seconds during which the token is valid.
* `sign_response` - (Optional) Boolean. Indicates whether or not the SAML Response should be signed instead of the SAML Assertion.
* `typed_attributes` - (Optional) Boolean, (Default=true). Indicates whether or not we should infer the `xs:type` of the element. Types include `xs:string`, `xs:boolean`, `xs:double`, and `xs:anyType`. When set to false, all `xs:type` are `xs:anyType`.
* `include_attribute_name_format` - (Optional) Boolean,(Default=true). Indicates whether or not we should infer the NameFormat based on the attribute name. If set to false, the attribute NameFormat is not set in the assertion.
* `name_identifier_format` - (Optional) String, (Default=`urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`). Format of the name identifier.
* `authn_context_class_ref` - (Optional) String. Class reference of the authentication context.
* `binding` - (Optional) String. Protocol binding used for SAML logout responses.
* `mappings` - (Optional) Map(String). Mappings between the Auth0 user profile property name (`name`) and the output attributes on the SAML attribute in the assertion (`value`).
* `logout` - (Optional) Map(Resource). Configuration settings for logout. For details, see [Logout](#logout).
* `name_identifier_probes` - (Optional) List(String). Attributes that can be used for Subject/NameID. Auth0 will try each of the attributes of this array in order and use the first value it finds.
* `signing_cert` - (Optional) String. Optionally indicates the public key certificate used to validate SAML requests. If set, SAML requests will be required to be signed. A sample value would be `-----BEGIN PUBLIC KEY-----\nMIGf...bpP/t3\n+JGNGIRMj1hF1rnb6QIDAQAB\n-----END PUBLIC KEY-----\n`.


#### Logout

`logout` supports the following options:

* `callback` - (Optional) String. Service provider's Single Logout Service URL, to which Auth0 will send logout requests and responses.
* `slo_enabled` - (Optional) Boolean. Indicates whether or not Auth0 should notify service providers of session termination.

### Mobile

`mobile` supports the following arguments:

* `android` (Optional) List(Resource). Configuration settings for Android native apps. For details, see [Android](#android).
* `ios` (Optional) List(Resource). Configuration settings for i0S native apps. For details, see [iOS](#ios).

#### Android

`android` supports the following arguments:

* `app_package_name` (Optional) String
* `sha256_cert_fingerprints` (Optional) List(String)

#### iOS

`ios` supports the following arguments:

* `team_id` - (Optional) String
* `app_bundle_identifier` - (Optional) String

### Native Social Login

> Note: once this is set it must stay set, with both resources set to "false" in order to change the app_type

`native_social_login` supports the following arguments:

* `apple` (Optional) Resource:
* `facebook` (Optional) Resources:

#### Apple

`apple` supports the following arguments:

* `enabled` Boolean

#### Facebook

`facebook` supports the following arguments:

* `enabled` Boolean

## Attribute Reference

Attributes exported by this resource include:

* `client_id` - String. ID of the client.
* `client_secret`<sup>[1](#client-keys)</sup> - String. Secret for the client; keep this private.
* `is_first_party` - Boolean. Indicates whether this client is a first-party client.
* `is_token_endpoint_ip_header_trusted` - Boolean
* `oidc_conformant` - Boolean. Indicates whether this client will conform to strict OIDC specifications.
* `grant_types` - List(String). Types of grants that this client is authorized to use.
* `custom_login_page_on` - Boolean. Indicates whether a custom login page is to be used.
* `token_endpoint_auth_method` - String. Defines the requested authentication method for the token endpoint. Options include `none` (public client without a client secret), `client_secret_post` (client uses HTTP POST parameters), `client_secret_basic` (client uses HTTP Basic).
* `signing_keys` - List(Map). List containing a map of the public cert of the signing key and the public cert of the signing key in pkcs7.

### Client keys

To access the `client_secret` attribute you need to add the `read:client_keys` scope to the Terraform client.
Otherwise, the attribute will contain an empty string.

## Import

A client can be imported using the client's ID, e.g.

```shell
$ terraform import auth0_client.my_client AaiyAPdpYdesoKnqjj8HJqRn4T5titww
```
