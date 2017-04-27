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

| HTTP  Method | **/customers** |  **/customers/{id}**
| ------ | ------ | ------ |
| GET | 200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists. | 200 (OK), single customer. 404 (Not Found), if ID not found or invalid. | 
| POST |404 (Not Found), unless you want to update/replace every resource in the entire collection. | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.|
| PUT | [ 404 (Not Found), unless you want to update/replace every resource in the entire collection.  | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.|
| DELETE | 404 (Not Found), unless you want to delete the whole collection—not often desirable. | 200 (OK). 404 (Not Found), if ID not found or invalid. | 

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
 

 
 
