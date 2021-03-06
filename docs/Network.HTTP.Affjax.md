## Module Network.HTTP.Affjax

#### `AJAX`

``` purescript
data AJAX :: !
```

The effect type for AJAX requests made with Affjax.

#### `Affjax`

``` purescript
type Affjax e a = Aff (ajax :: AJAX | e) (AffjaxResponse a)
```

The type for Affjax requests.

#### `AffjaxRequest`

``` purescript
type AffjaxRequest a = { method :: Method, url :: URL, headers :: Array RequestHeader, content :: Maybe a, username :: Maybe String, password :: Maybe String }
```

#### `defaultRequest`

``` purescript
defaultRequest :: AffjaxRequest Unit
```

#### `AffjaxResponse`

``` purescript
type AffjaxResponse a = { status :: StatusCode, headers :: Array ResponseHeader, response :: a }
```

The type of records that will be received as an Affjax response.

#### `URL`

``` purescript
type URL = String
```

Type alias for URL strings to aid readability of types.

#### `affjax`

``` purescript
affjax :: forall e a b. (Requestable a, Respondable b) => AffjaxRequest a -> Affjax e b
```

Makes an `Affjax` request.

#### `get`

``` purescript
get :: forall e a. (Respondable a) => URL -> Affjax e a
```

Makes a `GET` request to the specified URL.

#### `post`

``` purescript
post :: forall e a b. (Requestable a, Respondable b) => URL -> a -> Affjax e b
```

Makes a `POST` request to the specified URL, sending data.

#### `post'`

``` purescript
post' :: forall e a b. (Requestable a, Respondable b) => URL -> Maybe a -> Affjax e b
```

Makes a `POST` request to the specified URL with the option to send data.

#### `post_`

``` purescript
post_ :: forall e a. (Requestable a) => URL -> a -> Affjax e Unit
```

Makes a `POST` request to the specified URL, sending data and ignoring the
response.

#### `post_'`

``` purescript
post_' :: forall e a. (Requestable a) => URL -> Maybe a -> Affjax e Unit
```

Makes a `POST` request to the specified URL with the option to send data,
and ignores the response.

#### `put`

``` purescript
put :: forall e a b. (Requestable a, Respondable b) => URL -> a -> Affjax e b
```

Makes a `PUT` request to the specified URL, sending data.

#### `put'`

``` purescript
put' :: forall e a b. (Requestable a, Respondable b) => URL -> Maybe a -> Affjax e b
```

Makes a `PUT` request to the specified URL with the option to send data.

#### `put_`

``` purescript
put_ :: forall e a. (Requestable a) => URL -> a -> Affjax e Unit
```

Makes a `PUT` request to the specified URL, sending data and ignoring the
response.

#### `put_'`

``` purescript
put_' :: forall e a. (Requestable a) => URL -> Maybe a -> Affjax e Unit
```

Makes a `PUT` request to the specified URL with the option to send data,
and ignores the response.

#### `delete`

``` purescript
delete :: forall e a. (Respondable a) => URL -> Affjax e a
```

Makes a `DELETE` request to the specified URL.

#### `delete_`

``` purescript
delete_ :: forall e. URL -> Affjax e Unit
```

Makes a `DELETE` request to the specified URL and ignores the response.

#### `RetryDelayCurve`

``` purescript
type RetryDelayCurve = Int -> Int
```

A sequence of retry delays, in milliseconds.

#### `RetryPolicy`

``` purescript
type RetryPolicy = { timeout :: Maybe Int, delayCurve :: RetryDelayCurve, shouldRetryWithStatusCode :: StatusCode -> Boolean }
```

Expresses a policy for retrying Affjax requests with backoff.

#### `defaultRetryPolicy`

``` purescript
defaultRetryPolicy :: RetryPolicy
```

A sensible default for retries: no timeout, maximum delay of 30s, initial delay of 0.1s, exponential backoff, and no status code triggers a retry.

#### `retry`

``` purescript
retry :: forall e a b. (Requestable a) => RetryPolicy -> (AffjaxRequest a -> Affjax (avar :: AVAR, ref :: REF | e) b) -> AffjaxRequest a -> Affjax (avar :: AVAR, ref :: REF | e) b
```

Retry a request using a `RetryPolicy`. After the timeout, the last received response is returned; if it was not possible to communicate with the server due to an error, then this is bubbled up.

#### `affjax'`

``` purescript
affjax' :: forall e a b. (Requestable a, Respondable b) => AffjaxRequest a -> (Error -> Eff (ajax :: AJAX | e) Unit) -> (AffjaxResponse b -> Eff (ajax :: AJAX | e) Unit) -> Eff (ajax :: AJAX | e) (Canceler (ajax :: AJAX | e))
```

Run a request directly without using `Aff`.


