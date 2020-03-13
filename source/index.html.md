---
title: MTAA API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='//marekmansell.sk'>Marek Mansell</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

Welcome to the SocialN API! You can use our API to access SocialN API endpoints, which can get, change and add social network posts, user profiles, etc.

SocialN API is a simple RESTful application interface implemented in Python's Flask framework, which provides data synchronization and data storage.

SocialN API is used by the SocialN mobile app to share social media posts and images.

We have language bindings in Shell! You can view code examples in the dark area to the right.

## Version
API version of this documentation is `v1`, this is the latest API documentation for SocialN.


# Authentication

The SocialN API uses the password of the User to authenticate. If this would be a real life application, we would use an API token to allow users to authenticate without exposing their credentials.


Your password needs to be included in the Authorization header so your requests can be authenticated.

## Confirm you can connect

This endpoint will confirm you are authenticated properly.


> To authorize, use this code:

```shell
curl --request GET \
  --url 'https://mtaa.marekmansell.sk/v1/hello' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \

```
> The above command returns JSON structured like this:

```json
{
    "authenticated": true,
    "access_token": "*ab1C",
}

```

GET `https://mtaa.marekmansell.sk/v1/hello`

The access_token is the last four characters of whatever password you have used.


Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: YOUR-PASSWORD`

<aside class="notice">
You must replace <code>YOUR-PASSWORD</code> with your password.
</aside>

# Headers


All API requests must send the following request headers:

* `Authorization` with the value `Token token=YOUR-PASSWORD` where `YOUR-PASSWORD` is replaced with your password.
* `Accept` header with the value of `application/json`


# Pagination

Endpoints which enumerate objects are paginated.

Pagination information is returned in the meta attribute in the response body.

* `current_page` - The current page number.
* `next_page` - The next page number.
* `prev_page` - The previous page number.
* `total_pages` - The number of pages.
* `total_count` - The number of records with filtering applied,
* `per_page` - The number of results returned in each page.
* `overall_total` - The overall number of records without any filtering applied.

A specific page can be retrieved by providing the page parameter:

`https://mtaa.marekmansell.sk/v1/:user/posts?page=2`

# Errors

```json
{
  "errors": {
    "title": [
      "can't be blank"
    ]
  }
}
```
The SocialN API uses conventional HTTP response codes to indicate errors, and includes more detailed information on the exact nature of an error in the HTTP response.

For example, if a resource requires the title attribute to be present there will be an errors key in the response with subkey of title containing an array of errors.

## HTTP Status Codes

Code						| Meaning
--------------------------- | -----------
`200` OK						| All is well.
`201` Created					| Your request has been fulfilled and has resulted in one or more new resources being created.
`204` No Content				| Your request has been fulfilled and that there is no additional content to send in the response payload body. e.g. deleting a resource.
`401` Unauthorized			| Your request is not authenticated. See the Authentication Errors section for more information.
`404` Page Not Found			| The endpoint requested does not exist.
`422` Unprocessable Entity	| The resource couldn't be created or updated due to a validation error. Please see the response body for more information.
`429` Too Many Requests		| Your application is exceeding its rate limit. Slow down, pal!
`500` Internal Server Error	| Something is wrong on our end.
`504` Gateway Timeout			| Something has timed out on our end.

## Authentication Errors

```json
{
  "errors": {
    "password": [
      "is blank"
    ]
  },
  "hint": "You can retrieve your password by asking the administrator of the SocialN platform."
}
```
If a request cannot be authenticated the SocialN API returns the standard 401 unauthorized HTTP status code and includes more detailed information on the exact nature of the authentication error in the HTTP response.

The SocialN API includes a machine readable `errors` key. It also has a `hint` key containing human friendly information on how to resolve your authentication error.



# Users

Users represent the data which is connected directly to a user account, such as a password, biography, profile photo, etc.

## Attributes

Attribute				| Type			| Description
----------------------- | ------------- | -----------
`username` 				| string		| The user name used for login and API requests (e.g. "marekmansell")
`formatted_name` 		| string		| The formatted user name showed on profile/posts (e.g. "Marek Mansell") 
`password` 				| string		| The API password
`bio` 					| string		| The personal biography that shows on the profile page of a user
`photo` 				| string		| The user's profile photo 
`web` 					| url			| The user's website url
`mail` 					| string		| The user's email address
`date_of_birth` 		| date			| The user's date of birth

## Get All Users

```shell
curl --request GET \
  --url 'https://mtaa.marekmansell.sk/v1/users' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \

```

Retrieves all users.

`GET https://mtaa.marekmansell.sk/v1/users`


## Get a User

```shell
curl --request GET \
  --url 'https://mtaa.marekmansell.sk/v1/user/:username' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \

```

Retrieves a specific user by username.

`GET https://mtaa.marekmansell.sk/v1/user/:username`
* `username`  - The user name used for login and API requests (e.g. "marekmansell")

## Update a User

```shell
curl --request PATCH \
  --url 'https://mtaa.marekmansell.sk/v1/user/:username' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \
  --header 'Content-type: application/json' \
  --data '{"user":{"bio":"New bio aout the user..."}}'
```

Updates a specific user by username.

`GET https://mtaa.marekmansell.sk/v1/user/:username`
* `username`  - The user name used for login and API requests (e.g. "marekmansell")




# Posts

Posts represent individual SocialN posts which include a text description of up to 150 character and an optional image. Each post is linked to it's author.

## Attributes

Attribute				| Type			| Description
----------------------- | ------------- | -----------
`username` 				| string		| The user name used for login and API requests (e.g. "marekmansell")
`formatted_name` 		| string		| The formatted user name showed on profile/posts (e.g. "Marek Mansell") 
`publish_time` 			| datetime		| The time at which the post has been published
`photo` 				| image			| The image attached to the post (optional)
`content` 				| string		| The actual content of the post (max 150 characters)
`last_edit_time` 		| datetime		| The time at which the post has been updated (only most recent change)

## Get All Posts

```shell
curl --request GET \
  --url 'https://mtaa.marekmansell.sk/v1/posts' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \

```

Retrieves all posts from all users.

`GET https://mtaa.marekmansell.sk/v1/posts`

## Get a Post

```shell
curl --request GET \
  --url 'https://mtaa.marekmansell.sk/v1/post/:post_id' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \

```

Retrieves a specific post by id.

`GET https://mtaa.marekmansell.sk/v1/post/:post_id`
* `post_id`  - The unique post id for the searched post

## Update a Post

```shell
curl --request PATCH \
  --url 'https://mtaa.marekmansell.sk/v1/post/:post_id' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \
  --header 'Content-type: application/json' \
  --data '{"post":{"content":"New content..."}}'
```

Updates a specific post by post_id.

`GET https://mtaa.marekmansell.sk/v1/post/:post_id`
* `post_id`  - The unique post id for the searched post


## Delete a Post

```shell
curl --request DELETE \
  --url 'https://mtaa.marekmansell.sk/v1/post/:post_id' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \
```

Delete a specific post by post_id.

`GET https://mtaa.marekmansell.sk/v1/post/:post_id`
* `post_id`  - The unique post id for the searched post


## Create a Post

```shell
# Example without an image
curl --request POST \
  --url 'https://mtaa.marekmansell.sk/v1/post/' \
  --header 'Authorization: Token token=YOUR-PASSWORD' \
  --header 'Accept: application/json' \
  --header 'Content-type: application/json' \
  --data '{"post":{"content":"Very new post..."}}'
```

Create a new post

`GET https://mtaa.marekmansell.sk/v1/post/`

