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

*E.g, using CURL for anonymous users*
```bash
curl "https://api.href.ly?aid=d5e06094-dc00-4c70-8d2d-41ddc798c469" \
  -H "Content-Type: application/json"
```

*E.g, using CURL for registed users*
```bash
curl "https://api.href.ly" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your-token>"
```

*Server Response* `application/json` `200`
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

```yaml
uri: https://api.href.ly
method: post
authentication: everyone
```

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

[⬆ Top](#table-of-contents)
