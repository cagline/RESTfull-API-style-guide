# URI Format

## Rule: Forward slash separator (/) must be used to indicate a hierarchical relationship

The forward slash (/) character is used in the path portion of the URI to indicate a hierarchical relationship between resources. For example:
````sh 
api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares
````

## Rule: A trailing forward slash (/) should not be included in URIs

As the last character within a URI’s path, a forward slash (/) adds no semantic value and may cause confusion. REST APIs should not expect a trailing slash .

```sh 
http://api.canvas.restapi.org/shapes/
http://api.canvas.restapi.org/shapes
```

REST API must generate and communicate clean URIs and should be intolerant of any client’s attempts to identify a resource imprecisely.

## Rule: Hyphens (-) should be used to improve the readability of URIs

To make your URIs easy for people to scan and interpret, use the hyphen (-) character to improve the readability of names in long path segments. Anywhere you would use a space or hyphen in English, you should use a hyphen in a URI. For example:
```sh 
api.example.restapi.org/blogs/mark-masse/entries/this-is-my-first-post
```
## Rule: Underscores (_) should not be used in URIs 

Text viewer applications (browsers, editors, etc.) often underline URIs to provide a visual cue that they are clickable. Depending on the application’s font, the underscore (_) character can either get partially obscured or completely hidden by this underlying. 

## Rule: Lowercase letters should be preferred in URI paths

When convenient, lowercase letters are preferred in URI paths since capital letters can sometimes cause problems. RFC 3986 defines URIs as case-sensitive except for the scheme and host components. For example:

```sh 
http://api.example.restapi.org/my-folder/my-doc
HTTP://API.EXAMPLE.RESTAPI.ORG/my-folder/my-doc
http://api.example.restapi.org/My-Folder/my-doc
````

## Rule: File extensions should not be included in URIs 

A REST API should not include artificial file extensions in URIs to indicate the format of a message’s entity body. Instead, they should rely on the media type, as communicated through the Content-Type header, to determine how to process the body’s content.  

```sh
api.college.restapi.org/students/3248234/transcripts/2005/fall.json
api.college.restapi.org/students/3248234/transcripts/2005/fall
```

To enable simple links and easy debugging, a REST API may support media type selection via a query parameter as discussed in next slide

## Rule: Media type selection using a query parameter may be supported 

To enable simple links and easy debugging, REST APIs may support media type selection via a query parameter named accept with a value format that mirrors that of the Accept HTTP request header. 
For example: 
```sh
GET /bookmarks/mikemassedotcom?accept=application/xml
```
# URI Authority Design
## Rule: Consistent subdomain names should be used for your APIs 

The top-level domain and first subdomain names (e.g., soccer.restapi.org) of an API should identify its service owner. The full domain name of an API should add a subdomain named api. 
For example: 
```sh
http://api.soccer.restapi.org
```` 
## Rule: Consistent subdomain names should be used for your client developer portal 

Many REST APIs have an associated website, known as a developer portal, to help onboard new clients forums, and self-service provisioning of secure API access keys. 

For example: 
```sh
http://developer.soccer.restapi.org
```
# Resource Modeling

The URI path conveys a REST API’s resource model, with each forward slash separated path segment corresponding to a unique resource within the model’s hierarchy. 

For example, this URI design: 
```sh
api.cricket.restapi.org/leagues/ipl/teams/mumbai-indians
```
indicates that each of these URIs should also identify an addressable resource: 
```sh
http://api.cricket.restapi.org/leagues/ipl/teams 
http://api.soccer.restapi.org/leagues/ipl 
http://api.soccer.restapi.org/leagues 
http://api.soccer.restapi.org 
```
Resource modeling is an exercise that establishes your API’s key concepts. This process is similar to the data modeling for a relational database schema or the classical modeling of an object-oriented system.

# Resource Archetypes

API is composed of four distinct resource archetypes:

- Document
- Collection
- Store
- Controller

## Document

Each URI below identifies a document resource: 
```sh 
api.cricket.restapi.org/leagues/ipl
api.cricket.restapi.org/leagues/seattle/teams/mumbai-indians
api.cricket.restapi.org/leagues/ipl/teams/mumbai-indians/players/lasith-malinga
```
## Collection 

Each URI below identifies a collection resource: 

```sh 
api.cricket.restapi.org/leagues
api.cricket.restapi.org/leagues/ipl/teams
api.cricket.restapi.org/leagues/ipl/teams/mumbai-indians/players 
```
## Example for Document & Collection

| HTTP  Method | URL design | Description | HTTP response code |
| ------ | ------ | ------ |------ |
| **GET** | **api.bank.lk/v1/banks** | **Get list of collection** |
| **POST** | **api.bank.lk/v1/banks** | **Create a document** ***(bank)*** |
| PUT | api.bank.lk/v1/banks |  Update/replace entire collection *(not often desirable)* |
| DELETE | api.bank.lk/v1/banks | **Delete the whole collection *(not often desirable)* |
| **GET** | **api.bank.lk/v1/banks/{id}** | **Get one document** |
| POST | api.bank.lk/v1/banks | N/A |
| **PUT** | **api.bank.lk/v1/banks/{id}** | **Update one resource** |
| **DELETE** | **api.bank.lk/v1/banks/{id}** | **Delete the one document** |




## Recap for Document & Collection

| HTTP  Method | **/customers** |  **/customers/{id}**
| ------ | ------ | ------ |
| GET | 200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists. | 200 (OK), single customer. 404 (Not Found), if ID not found or invalid. | 
| POST |201 (Created), 'Location' header with link to/customers/{id} containing new ID. | 404 (Not Found).|
| PUT | 404 (Not Found), unless you want to update/replace every resource in the entire collection.  | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.|
| DELETE | 404 (Not Found), unless you want to delete the whole collection—not often desirable. | 200 (OK). 404 (Not Found), if ID not found or invalid. | 



## HTTP request method summary

| HTTP  Method | Semantics |
| ------ | ------ |
| DELETE  | HTTP request method used to remove its parent.|
| GET  | HTTP request method used to retrieve a representation of a resource’s state.|
| HEAD  | HTTP request method used to retrieve the metadata associated with the resource’s state.|
| OPTIONS  | HTTP request method used to retrieve metadata that describes a resource’s available interactions.|
| POST  | HTTP request method used to create a new resource within a collection or execute a controller.|
| PUT  | HTTP request method used to insert a new resource into a store or update a mutable resource.|

 

# Hypermedia as the engine of application state (HATEOAS)
### Github API Examaple
Request
``` sh
GET https://api.github.com/users/cagline
```
Response
```sh
{
  "login": "cagline",
  "id": 6813939,
  "avatar_url": "https://avatars3.githubusercontent.com/u/6813939?v=3",
  "gravatar_id": "",
  "url": "https://api.github.com/users/cagline",
  "html_url": "https://github.com/cagline",
  "followers_url": "https://api.github.com/users/cagline/followers",
  "following_url": "https://api.github.com/users/cagline/following{/other_user}",
  "gists_url": "https://api.github.com/users/cagline/gists{/gist_id}",
  "starred_url": "https://api.github.com/users/cagline/starred{/owner}{/repo}",
  "subscriptions_url": "https://api.github.com/users/cagline/subscriptions",
  "organizations_url": "https://api.github.com/users/cagline/orgs",
  "repos_url": "https://api.github.com/users/cagline/repos",
  "events_url": "https://api.github.com/users/cagline/events{/privacy}",
  "received_events_url": "https://api.github.com/users/cagline/received_events",
  "type": "User",
  "site_admin": false,
  "name": "Chanuka Asanka",
  "company": "CAGLine Solution",
  "blog": "www.cagline.info",
  "location": "Sri Lanka",
  "email": "cagline@gmail.com",
  "hireable": true,
  "bio": null,
  "public_repos": 6,
  "public_gists": 0,
  "followers": 0,
  "following": 1,
  "created_at": "2014-02-28T08:57:40Z",
  "updated_at": "2017-04-26T16:57:02Z"
}
```
