# GitHub Repositories API Reference

## Table of Contents
- [Overview](#overview)
- [Authentication](#authentication) 
  - [Getting a Personal Access Token](#getting-a-personal-access-token)
  - [Using Your Token](#using-your-token)
  - [Unauthenticated Access](#unauthenticated-access)
- [Endpoints](#endpoints)
  - [List Repositories for Authenticated User](#list-repositories-for-authenticated-user)
  - [Get a Repository](#get-a-repository)
- [Error Handling](#error-handling) 
  - [Common Errors](#common-errors) 
  - [Handling Errors in Code](#handling-errors-in-code)
- [Best Practices](#best-practices)
  - [Rate Limit Management](#rate-limit-management)
  - [Pagination](#pagination)
  - [Secure Token Storage](#secure-token-storage)
  - [Request Timeouts](#request-timeouts)  
  - [Caching](#caching)
- [Conclusion](#conclusion)

## Overview

The GitHub Repositories API enables you to create, read, update, and manage repositories programmatically. This reference covers the primary repository endpoints for listing, retrieving, creating, and updating repositories.

**Base URL:** `https://api.github.com`

**API Version:** REST API v3

**Authentication:** Most endpoints require a Personal Access Token (PAT) passed in the `Authorization` header.

**Rate Limits:**
- Unauthenticated requests: 60 requests per hour
- Authenticated requests: 5,000 requests per hour

The rate limit is provided in the response headers:
- `X-RateLimit-Limit`: Total requests allowed per hour
- `X-RateLimit-Remaining`: Requests remaining in the current window
- `X-RateLimit-Reset`: Unix timestamp when the limit resets

## Authentication
Most repository endpoints require authentication using a Personal Access Token (PAT).
### Getting a Personal Access Token
1. Go to **GitHub Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token (classic)**
3. Give your token a descriptive name
4. Select the `repo` scope for full repository access
5. Click **Generate token** and copy it immediately (you won't see it again)
### Using Your Token
Include your token in the `Authorization` header of every request:
```bash
Authorization: token YOUR_PERSONAL_ACCESS_TOKEN
```
### Example Request with Authentication
```python
import requests
headers = {
    "Authorization": "token ghp_xxxxxxxxxxxxxxxxxxxx",
    "Accept": "application/vnd.github+json"
}
response = requests.get("https://api.github.com/user/repos", headers=headers)
```
> ⚠️ **Security Warning:** 
Never commit tokens to public repositories or hardcode them in your code for security reasons. Use environment variables or secrets manager to store tokens safely.
### Unauthenticated Access
Some endpoints allow unauthenticated access to public data, but with strict rate limits (60 requests/hour).

## Endpoints
### List Repositories for Authenticated User
Returns a list of repositories owned by the authenticated user.
**Endpoint**
```
GET /user/repos
```
**Authentication:** Required

**Parameters**
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `visibility` | string | No | `all` | Filter by visibility: `all`, `public`, or `private` |
| `affiliation` | string | No | `owner,collaborator,organization_member` | Filter by affiliation: `owner`, `collaborator`, `organization_member` |
| `type` | string | No | `all` | Filter by type: `all`, `owner`, `public`, `private`, `member` |
| `sort` | string | No | `full_name` | Sort by: `created`, `updated`, `pushed`, `full_name` |
| `direction` | string | No | `asc` | Sort direction: `asc` or `desc` |
| `per_page` | integer | No | `30` | Results per page (max 100) |
| `page` | integer | No | `1` | Page number for pagination |

**Example Request**
```python
import requests

headers = {    
    "Authorization": "token YOUR_TOKEN",
    "Accept": "application/vnd.github+json"
}

params = {
    "visibility": "public",
    "sort": "updated",
    "per_page": 10
}

response = requests.get(
    "https://api.github.com/user/repos",
    headers=headers,
    params=params
)
    
repos = response.json()
```
**Example Response**
```json
[
  {
      "id": 641548326,
      "name": "AI-blogger",
      "full_name": "Shruti-Rajpurohit/AI-blogger",
      "private": true,
      "owner": {
          "login": "Shruti-Rajpurohit",
          "id": 123017104
      },
      "html_url": "https://github.com/Shruti-Rajpurohit/AI-blogger",
      "description": null,
      "fork": false,
      "created_at": "2023-05-16T17:47:10Z",
      "updated_at": "2023-05-16T18:00:09Z",
      "pushed_at": "2023-05-27T13:22:33Z",
      "size": 24,
      "stargazers_count": 0,
      "language": "Python",
      "forks_count": 0,
      "open_issues_count": 0,
      "default_branch": "main"
  }
]
```
**Response Fields**
| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique repository ID |
| `name` | string | Repository name |
| `full_name` | string | Full repository name including owner |
| `private` | boolean | Whether the repository is private |
| `html_url` | string | Web URL to view the repository |
| `description` | string | Repository description (can be null) |
| `created_at` | string | ISO 8601 timestamp of creation |
| `updated_at` | string | ISO 8601 timestamp of last update |
| `language` | string | Primary programming language |
| `stargazers_count` | integer | Number of stars |
| `forks_count` | integer | Number of forks |

**Status Codes**
| Code | Description |
|------|-------------|
| `200` | Success - Returns array of repositories |
| `401` | Unauthorized - Invalid or missing authentication token |
| `403` | Forbidden - Rate limit exceeded |

### Get a Repository
Retrieves detailed information about a specific repository.
**Endpoint**
```
GET /repos/{owner}/{repo}
```
**Authentication:** Optional (required for private repositories)

**Path Parameters**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | Yes | Repository owner's username |
| `repo` | string | Yes | Repository name |

**Example Request**
```python
import requests

# Public repository (no authentication needed)
response = requests.get("https://api.github.com/repos/octocat/Hello-World")

# Private repository (authentication required)
headers = {
    "Authorization": "token YOUR_TOKEN",
    "Accept": "application/vnd.github+json"
}

response = requests.get(
    "https://api.github.com/repos/Shruti-Rajpurohit/AI-blogger",
    headers=headers
)

repo = response.json()
print(f"Repository: {repo['name']}")
print(f"Stars: {repo['stargazers_count']}")
print(f"Language: {repo['language']}")
```
**Example Response**
```json
{
    "id": 641548326,
    "name": "AI-blogger",
    "full_name": "Shruti-Rajpurohit/AI-blogger",
    "private": true,
    "owner": {
        "login": "Shruti-Rajpurohit",
        "id": 123017104,
        "avatar_url": "https://avatars.githubusercontent.com/u/123017104?v=4"
    },
    "html_url": "https://github.com/Shruti-Rajpurohit/AI-blogger",
    "description": null,
    "fork": false,
    "created_at": "2023-05-16T17:47:10Z",
    "updated_at": "2023-05-16T18:00:09Z",
    "pushed_at": "2023-05-27T13:22:33Z",
    "size": 24,
    "stargazers_count": 0,
    "watchers_count": 0,
    "language": "Python",
    "forks_count": 0,
    "open_issues_count": 0,
    "license": {
        "key": "mit",
        "name": "MIT License",
        "url": "https://api.github.com/licenses/mit"
    },
    "topics": [],
    "visibility": "private",
    "default_branch": "main"
}
```
**Status Codes**
| Code | Description |
|------|-------------|
| `200` | Success - Returns repository object |
| `404` | Not Found - Repository doesn't exist or is private without authentication |
| `403` | Forbidden - Rate limit exceeded |

## Error Handling
The GitHub API returns standard HTTP status codes and structured error responses in JSON format.
### Error Response Format
```json
{
  "message": "Not Found",
  "documentation_url": "https://docs.github.com/rest/repos/repos#get-a-repository",
  "status": "404"
}
```
### Common Errors
**401 Unauthorized - Missing Authentication**
```json
{
  "message": "Requires authentication",
  "documentation_url": "https://docs.github.com/rest"
}
```
**Cause:** 
Trying to access an authenticated endpoint without a token, or attempting to access a private repository without valid credentials.

**Solution:** 
Include a valid Personal Access Token in the `Authorization` header.

---
**401 Unauthorized - Invalid Token**
```json
{
  "message": "Bad credentials",
  "documentation_url": "https://docs.github.com/rest"
}
```
**Cause:** 
Token has expired, been revoked, or is malformed.
**Solution:** 
Generate a new Personal Access Token and update your application.

---
**404 Not Found**
```json
{
  "message": "Not Found",
  "documentation_url": "https://docs.github.com/rest"
}
```
**Cause:** 
Repository does not exist, has been deleted, or is private and you lack access.
**Solution:** 
Double-check the owner and repository names. For private repositories, ensure your token has appropriate permissions.

---
**403 Forbidden - Rate Limit Exceeded**
```json
{
  "message": "API rate limit exceeded",
  "documentation_url": "https://docs.github.com/rest/overview/resources-in-the-rest-api#rate-limiting"
}
```
**Cause:** 
You have reached your hourly rate limit (60 for unauthenticated, 5000 for authenticated requests).
**Solution:** 
Wait until your rate limit resets (check `X-RateLimit-Reset` header) or authenticate your requests if you haven't already.

---
**422 Unprocessable Entity**
```json
{
  "message": "Validation Failed",
  "errors": [
    {
      "resource": "Repository",
      "field": "name",
      "code": "already_exists"
    }
  ]
}
```
**Cause:** 
Request body contains invalid data (e.g., creating a repository with a name that already exists).
**Solution:** 
Review the errors array to identify which fields failed validation and correct them.
### Handling Errors in Code
```python
import requests
headers = {"Authorization": "token YOUR_TOKEN"}
try:
    response = requests.get(
        "https://api.github.com/repos/owner/repo",
        headers=headers
    )
    response.raise_for_status()  # Raises exception for 4xx/5xx status codes
    repo = response.json()
   
except requests.exceptions.HTTPError as e:
    if response.status_code == 404:
        print("Repository not found")
    elif response.status_code == 401:
        print("Authentication failed - check your token")
    elif response.status_code == 403:
        print("Rate limit exceeded - wait before retrying")
    else:
        print(f"HTTP error: {e}")

except requests.exceptions.RequestException as e:
    print(f"Network error: {e}")
```

## Best Practices
### Rate Limit Management
Always check rate limit headers in responses to avoid hitting limits:
```python
response = requests.get(url, headers=headers)
remaining = response.headers.get('X-RateLimit-Remaining')
limit = response.headers.get('X-RateLimit-Limit')
reset_time = response.headers.get('X-RateLimit-Reset')

print(f"Rate limit: {remaining}/{limit}")
```
If you're approaching the limit, implement exponential backoff or wait until the reset time.
### Pagination
When fetching large lists, use pagination to avoid timeouts and reduce load:
```python
def get_all_repos():
    repos = []
    page = 1
    per_page = 100  # Maximum allowed
    while True:
        response = requests.get(
            "https://api.github.com/user/repos",
            headers=headers,
            params={"per_page": per_page, "page": page}
        )
        data = response.json()
        if not data:
            break
        repos.extend(data)
        page += 1
    return repos
```
### Secure Token Storage
Never hardcode tokens in your source code. Use environment variables:
```python
import os
token = os.environ.get("GITHUB_TOKEN")
if not token:
    raise ValueError("GITHUB_TOKEN environment variable not set")

headers = {"Authorization": f"token {token}"}
```
### Request Timeouts
Always set timeouts to prevent your application from hanging:
```python
response = requests.get(url, headers=headers, timeout=10)
```
### Caching
For data that doesn't change frequently, implement caching to reduce API calls:
```python
import time
cache = {}
CACHE_DURATION = 300  # 5 minutes
def get_repo_cached(owner, repo):
    key = f"{owner}/{repo}"
    if key in cache:
        data, timestamp = cache[key]
        if time.time() - timestamp < CACHE_DURATION:
            return data
    response = requests.get(f"https://api.github.com/repos/{owner}/{repo}")
    data = response.json()
    cache[key] = (data, time.time())

    return data
```

## Conclusion
This reference covers the core GitHub Repositories API endpoints for listing and retrieving repositories. The GitHub API offers many more endpoints for creating, updating, and managing repositories programmatically.

**Key Takeaways:**
- Always authenticate requests to increase rate limits from 60 to 5,000 per hour
- Handle errors gracefully by reviewing status codes and response messages
- Use pagination for large datasets
- Store tokens securely using environment variables
- Monitor rate limits to avoid service disruptions

**Additional Resources:**
- [Official GitHub REST API Documentation](https://docs.github.com/en/rest)
- [GitHub API Rate Limiting Guide](https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting)
- [GitHub Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

**Questions or Feedback?**
- GitHub: [Shruti-Rajpurohit](https://github.com/Shruti-Rajpurohit)
- LinkedIn: [Shruti Rajpurohit](https://www.linkedin.com/in/shruti-rajpurohit-976b44226)
- Email: rajpurohitshruti20@gmail.com
