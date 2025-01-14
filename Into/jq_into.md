# Intro 

JQ is a filter: 
1. it takes ***input, 
2. processs and 
3. output 

Foundamental concepts
1. How jq works 
2. Basic filters
3. Object identifier-index and Optional Object Identifier-index
4. Object Index and Array Index
5. Array/String Slice and Array/Object Value Iterator
6. Comma, Pipe and parenthesis 


## How jq wroks
jq filters run on <mark1>a stream of JSON data.</mark>

The input to jq is parsed as ***a sequence of whitespace-separated JSON values*** which are passed through the provided filter one at a time

The output(s) of the filter are ***written to standard output, as a sequence of newline-separated JSON data.***

## Basic filters

### Identity: <mark>***.***</mark>

The simplest and most common filter (or jq program) is <mark>.</mark>, which is the ***identity operator***, <mark>copying the inputs of the jq processor to the output stream. </mark>

Because the default behavior of the jq processor is to read JSON texts from the input stream, and to pretty-print outputs, the . program's main use is to validate and pretty-print the inputs.


but sometime the jq will losing precision 

```
1E1234567890 | . 

(return 1.7976931348623157e+308  jq has converted it to an IEEE754 double-precision representation, losing precision. on parsing this number)
```

the parsing procedures:

(1) Any arithmetic operation on a number that has not already been converted to an IEEE754 double precision representation will trigger a conversion to the IEEE754 representation.

(2) jq will attempt to maintain the original decimal precision of number literals, but in expressions such 1E1234567890, precision will be lost if the exponent is too large.

(3) In jq programs, a leading minus sign will trigger the conversion of the number to an IEEE754 representation.

(4) Comparisons are carried out using the untruncated big decimal representation of numbers if available, as illustrated in one of the following examples.

#### Identity example 

```
 echo '"Hello, world!"' | jq '.'
```

```
{
  "id": 5101141,
  "node_id": "MDEwOlJlcG9zaXRvcnk1MTAxMTQx",
  "name": "jq",
  "full_name": "jqlang/jq",
  "private": false,
  "owner": {
    "login": "jqlang",
    "id": 104800540,
    "node_id": "O_kgDOBj8hHA",
    "avatar_url": "https://avatars.githubusercontent.com/u/104800540?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/jqlang",
    "html_url": "https://github.com/jqlang",
    "followers_url": "https://api.github.com/users/jqlang/followers",
    "following_url": "https://api.github.com/users/jqlang/following{/other_user}",
    "gists_url": "https://api.github.com/users/jqlang/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/jqlang/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/jqlang/subscriptions",
    "organizations_url": "https://api.github.com/users/jqlang/orgs",
    "repos_url": "https://api.github.com/users/jqlang/repos",
    "events_url": "https://api.github.com/users/jqlang/events{/privacy}",
    "received_events_url": "https://api.github.com/users/jqlang/received_events",
    "type": "Organization",
    "user_view_type": "public",
    "site_admin": false
  },
  "html_url": "https://github.com/jqlang/jq",
  "description": "Command-line JSON processor",
  "fork": false,
  "url": "https://api.github.com/repos/jqlang/jq",
  "forks_url": "https://api.github.com/repos/jqlang/jq/forks",
  "keys_url": "https://api.github.com/repos/jqlang/jq/keys{/key_id}",
  "collaborators_url": "https://api.github.com/repos/jqlang/jq/collaborators{/collaborator}",
  "teams_url": "https://api.github.com/repos/jqlang/jq/teams",
  "hooks_url": "https://api.github.com/repos/jqlang/jq/hooks",
  "issue_events_url": "https://api.github.com/repos/jqlang/jq/issues/events{/number}",
  "events_url": "https://api.github.com/repos/jqlang/jq/events",
  "assignees_url": "https://api.github.com/repos/jqlang/jq/assignees{/user}",
  "branches_url": "https://api.github.com/repos/jqlang/jq/branches{/branch}",
  "tags_url": "https://api.github.com/repos/jqlang/jq/tags",
  "blobs_url": "https://api.github.com/repos/jqlang/jq/git/blobs{/sha}",
  "git_tags_url": "https://api.github.com/repos/jqlang/jq/git/tags{/sha}",
  "git_refs_url": "https://api.github.com/repos/jqlang/jq/git/refs{/sha}",
  "trees_url": "https://api.github.com/repos/jqlang/jq/git/trees{/sha}",
  "statuses_url": "https://api.github.com/repos/jqlang/jq/statuses/{sha}",
  "languages_url": "https://api.github.com/repos/jqlang/jq/languages",
  "stargazers_url": "https://api.github.com/repos/jqlang/jq/stargazers",
  "contributors_url": "https://api.github.com/repos/jqlang/jq/contributors",
  "subscribers_url": "https://api.github.com/repos/jqlang/jq/subscribers",
  "subscription_url": "https://api.github.com/repos/jqlang/jq/subscription",
  "commits_url": "https://api.github.com/repos/jqlang/jq/commits{/sha}",
  "git_commits_url": "https://api.github.com/repos/jqlang/jq/git/commits{/sha}",
  "comments_url": "https://api.github.com/repos/jqlang/jq/comments{/number}",
  "issue_comment_url": "https://api.github.com/repos/jqlang/jq/issues/comments{/number}",
  "contents_url": "https://api.github.com/repos/jqlang/jq/contents/{+path}",
  "compare_url": "https://api.github.com/repos/jqlang/jq/compare/{base}...{head}",
  "merges_url": "https://api.github.com/repos/jqlang/jq/merges",
  "archive_url": "https://api.github.com/repos/jqlang/jq/{archive_format}{/ref}",
  "downloads_url": "https://api.github.com/repos/jqlang/jq/downloads",
  "issues_url": "https://api.github.com/repos/jqlang/jq/issues{/number}",
  "pulls_url": "https://api.github.com/repos/jqlang/jq/pulls{/number}",
  "milestones_url": "https://api.github.com/repos/jqlang/jq/milestones{/number}",
  "notifications_url": "https://api.github.com/repos/jqlang/jq/notifications{?since,all,participating}",
  "labels_url": "https://api.github.com/repos/jqlang/jq/labels{/name}",
  "releases_url": "https://api.github.com/repos/jqlang/jq/releases{/id}",
  "deployments_url": "https://api.github.com/repos/jqlang/jq/deployments",
  "created_at": "2012-07-18T19:57:25Z",
  "updated_at": "2025-01-13T11:38:26Z",
  "pushed_at": "2024-12-29T12:54:12Z",
  "git_url": "git://github.com/jqlang/jq.git",
  "ssh_url": "git@github.com:jqlang/jq.git",
  "clone_url": "https://github.com/jqlang/jq.git",
  "svn_url": "https://github.com/jqlang/jq",
  "homepage": "https://jqlang.github.io/jq/",
  "size": 10177,
  "stargazers_count": 30943,
  "watchers_count": 30943,
  "language": "C",
  "has_issues": true,
  "has_projects": false,
  "has_downloads": true,
  "has_wiki": true,
  "has_pages": true,
  "has_discussions": true,
  "forks_count": 1592,
  "mirror_url": null,
  "archived": false,
  "disabled": false,
  "open_issues_count": 460,
  "license": {
    "key": "other",
    "name": "Other",
    "spdx_id": "NOASSERTION",
    "url": null,
    "node_id": "MDc6TGljZW5zZTA="
  },
  "allow_forking": true,
  "is_template": false,
  "web_commit_signoff_required": false,
  "topics": [
    "jq"
  ],
  "visibility": "public",
  "forks": 1592,
  "open_issues": 460,
  "watchers": 30943,
  "default_branch": "master",
  "temp_clone_token": null,
  "custom_properties": {

  },
  "organization": {
    "login": "jqlang",
    "id": 104800540,
    "node_id": "O_kgDOBj8hHA",
    "avatar_url": "https://avatars.githubusercontent.com/u/104800540?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/jqlang",
    "html_url": "https://github.com/jqlang",
    "followers_url": "https://api.github.com/users/jqlang/followers",
    "following_url": "https://api.github.com/users/jqlang/following{/other_user}",
    "gists_url": "https://api.github.com/users/jqlang/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/jqlang/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/jqlang/subscriptions",
    "organizations_url": "https://api.github.com/users/jqlang/orgs",
    "repos_url": "https://api.github.com/users/jqlang/repos",
    "events_url": "https://api.github.com/users/jqlang/events{/privacy}",
    "received_events_url": "https://api.github.com/users/jqlang/received_events",
    "type": "Organization",
    "user_view_type": "public",
    "site_admin": false
  },
  "network_count": 1592,
  "subscribers_count": 325
}

```

To disply the jq :
```
cat example.json | jq '.'
```

if I want to get the ***value*** on specfic key 

```
cat example.json | jq '.name'
```

it returns 
```
"jq"
```

If the object is a array or jsonObject 

Array:
```
cat example.json | jq '.topics'
```

it will return an array 
```
[
  "jq"
]
```

if it is a jsonObject

```
cat example.json | jq '.organization'

```

it will return jsonObject

```
{
  "login": "jqlang",
  "id": 104800540,
  "node_id": "O_kgDOBj8hHA",
  "avatar_url": "https://avatars.githubusercontent.com/u/104800540?v=4",
  "gravatar_id": "",
  "url": "https://api.github.com/users/jqlang",
  "html_url": "https://github.com/jqlang",
  "followers_url": "https://api.github.com/users/jqlang/followers",
  "following_url": "https://api.github.com/users/jqlang/following{/other_user}",
  "gists_url": "https://api.github.com/users/jqlang/gists{/gist_id}",
  "starred_url": "https://api.github.com/users/jqlang/starred{/owner}{/repo}",
  "subscriptions_url": "https://api.github.com/users/jqlang/subscriptions",
  "organizations_url": "https://api.github.com/users/jqlang/orgs",
  "repos_url": "https://api.github.com/users/jqlang/repos",
  "events_url": "https://api.github.com/users/jqlang/events{/privacy}",
  "received_events_url": "https://api.github.com/users/jqlang/received_events",
  "type": "Organization",
  "user_view_type": "public",
  "site_admin": false
}
```

