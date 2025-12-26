# API Pagination
##

All Private Packagist API endpoints that return lists support pagination using query parameters. This allows you to efficiently retrieve large result sets by fetching data in smaller chunks.

> **✨ Automatic Pagination**: If you're using the [PHP API client](https://github.com/packagist/private-packagist-api-client), pagination is handled automatically! The client fetches all pages transparently and returns complete results. You don't need to manually handle pagination, Link headers, or page loops - just call the endpoint method and get all results.

If you were previously using list endpoints without pagination: Your existing code will continue to work, but the hard limit is set to 10,000 items, which covers most use cases.

## Query Parameters

Paginated list endpoints (all that do get a collection of items) accept the following query parameters:

* `page` (integer, optional): Page number to retrieve (1-10,000). Default: 1
* `limit` (integer, optional): Number of items per page (1-10,000). Default: 10,000

### Example Request

```bash
GET /api/packages/?page=2&limit=300
```

This fetches the second page of packages with 300 packages per page.

## Response Format

### Link Header

Paginated responses include an [RFC 5988](https://tools.ietf.org/html/rfc5988) Link header containing pagination navigation URLs:

```
Link: <https://packagist.com/api/packages/?page=3&limit=300>; rel="next",
      <https://packagist.com/api/packages/?page=1&limit=300>; rel="prev",
      <https://packagist.com/api/packages/?page=1&limit=300>; rel="first",
      <https://packagist.com/api/packages/?page=10&limit=300>; rel="last"
```

The Link header includes up to four relations:

* `next`: URL to the next page (if available)
* `prev`: URL to the previous page (if available)
* `first`: URL to the first page
* `last`: URL to the last page

### Response Body

The response body contains a JSON array of resources, just like non-paginated responses:

```json
[
  {
    "id": 1,
    "name": "acme/package",
    ...
  },
  ...
]
```
