## What was the bug?

In my `Client.request` method, when `oauth2_token` was stored as a plain dictionary, the client did not refresh the OAuth2 token and API requests could be sent without a valid `Authorization` header.

## Why did it happen?

The original refresh condition only checked for a missing token (`None`) or an expired `OAuth2Token` instance. When `oauth2_token` was a non-empty dict, that condition evaluated to false and skipped `refresh_oauth2`, even though the token data was stale and not an `OAuth2Token`.

## Why does my fix solve it?

My new condition refreshes whenever `oauth2_token` is not an `OAuth2Token` instance, or when an `OAuth2Token` is expired. This treats dicts and other unexpected types as invalid and ensures that API requests always have a fresh bearer token.

## One realistic edge case my tests do not cover

My tests do not cover the case where `oauth2_token` is a valid, unexpired `OAuth2Token` but the underlying access token is revoked server-side while still within its expiry window.
