# Href.ly API

This is the documentation for using the Href.ly API. Here you will discover the
steps to manage your shortened links, shorten new links and get their respective
metrics.

Keep in mind that some features are restricted depending on the subscription you
have, for more information, log in to your account and go to the plans and billing
section.

If there's anything you think we should add, please let us know at [support@href.ly](mailto:support@href.ly).


### Table of Contents

- [Headers](#headers)
- [Authentication](#authentication-for-anonymous-users)
  - [Anonymous](#authentication-for-anonymous-users)
  - [Registered users](#authentication-for-registered-users)
- [Links](#index-shortened-links)
  - [Index shortened links](#index-shortened-links)
  - [Short link](#short-link)


### Headers

All requests must have the following mandatory headers.

```json
{
  "Content-Type": "application/json"
}
```

### Authentication for anonymous users

To connect to our API has an anonymous user the `Authorization` header is not required.
Here you must send an `aid` (Anonymous User Identifier), in UUID format, this parameter
should be passed on the query request. Also, keep in mind that the requests are stateless,
which means that you will be in charge of storing this unique value and sending
it in each request to identify yourself.

E.g,
```bash
curl "https://api.href.ly?aid=d5e06094-dc00-4c70-8d2d-41ddc798c469" -H "Content-Type: application/json"
```

[⬆ Top](#table-of-contents)


### Authentication for registered users

To connect to our API has a subscribed user you must use the `Authorization` header.
This can be obtained from your account, under developer settings. This token has
full access to your shortened links, be careful how you use it in your apps.

[⬆ Top](#table-of-contents)


### Errors

When we detect an error in the request, we will return a HTTP `400`, authentication
errors will return a code `401`, authorization errors will return a code `403`, every
error will return a JSON payload with a single key `message` containing the error message.
If you aren't receiving errors, make sure you've set `"Accept: application/json"`
in your request headers.

E.g,
```json
{
  "message": "This action is unauthorized"
}
```

[⬆ Top](#table-of-contents)


#### Index shortened links

Get your shortened links.

```yaml
uri: https://api.href.ly
method: get
authentication: everyone
```

```bash
# E.g, using CURL for anonymous users
curl "https://api.href.ly?aid=d5e06094-dc00-4c70-8d2d-41ddc798c469" \
  -H "Content-Type: application/json"
```

```bash
# E.g, using CURL for registed users
curl "https://api.href.ly" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your-token>"
```

Server Response `application/json` `200`
```json
{
  "data":
  [
    {
      "id": 2,
      "hash": "20QSlQa",
      "long": "https://example.com/index.html",
      "short": "https://href.ly/20QSlQa",
      "tini": "href.ly/20QSlQa",
      "html": "<a href=\"https://href.ly/20QSlQa\" title=\"Link to an example page\">https://href.ly/20QSlQa</a>",
      "markdown": "[Link to an example page](https://href.ly/20QSlQa)",
      "customized": false,
      "description": "Link to an example page",
      "shots": 0,
      "created_at": "2022-11-03T00:32:53.000000Z"
    }
  ]
}
```

[⬆ Top](#table-of-contents)


#### Short link

Store and short a link.

```yaml
uri: https://api.href.ly
method: post
authentication: everyone
```

Request Content `application/json`

| Name          | Description                                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------------------ |
| `long`        | `required` `url` The long link that needs to be shortened.                                                         |
| `description` | `optional` `string` A description of the link you are shortening.                                                  |
| `aid`         | `optional` `uuid` Required if the `Authorization` header is not present.                                           |
| `hash`        | `optional` `string` If you are a registered user you can customize the short link slug. Ignored if aid is present. |

```bash
# E.g, using CURL for anonymous users
curl -X "POST" "https://api.href.ly" \
  -H "Content-Type: application/json" \
  -d '{"aid": "d5e06094-dc00-4c70-8d2d-41ddc798c469", "long": "https://example.com"}'
```

```bash
# E.g, using CURL for registered users
curl -X "POST" "https://api.href.ly" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your-token>" \
  -d '{"long": "https://example.com"}'
```

Server Response `application/json` `201`
```json
{
  "data": {
    "id": 3,
    "hash": "o1CzXyo",
    "long": "https://example.com",
    "short": "https://href.ly/o1CzXyo",
    "tini": "href.ly/o1CzXyo",
    "html": "<a href=\"https://href.ly/o1CzXyo\" title=\"https://href.ly/o1CzXyo\">https://href.ly/o1CzXyo</a>",
    "markdown": "[https://href.ly/o1CzXyo](https://href.ly/o1CzXyo)",
    "customized": false,
    "description": "",
    "shots": 0,
    "created_at": "2022-11-03T00:44:38.000000Z"
  }
}
```

[⬆ Top](#table-of-contents)
