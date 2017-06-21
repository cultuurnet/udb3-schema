# UDB3 Schema

[![Build Status](https://travis-ci.org/cultuurnet/udb3-schema.svg?branch=master)](https://travis-ci.org/cultuurnet/udb3-schema)

Swagger documentation for the udb3 api.

## Folder structure and file naming

The `swagger.json` file at the root of the project is bundled from content of `src/`. You should not edit this file manually. It is generated using the command-line by running `npm run bundle`. Each sub-folder of `src/` contains the files relevant to a specific domain like `user/` or `authorization/`.

We use the following file naming pattern to quicly identify object files: `thing.type.json`. Following this convention, the file describing the content of a `user/` response is named `user-information.response.json`.

## Contributing

Install the npm dependencies:

    npm install
    
Bundle and validate your changes:

    npm run bundle
    npm run validate
    
The same scripts run on Travis for every push event.

## Design Guidelines

The following guidelines should always be followed when making changes, even though existing docs may not always follow them.

**The examples in these guidelines do not necessarily reflect real endpoints.**

### Resource collections

Resource collections should always be plural, and end in a trailing slash because they represent a directory of resources.

Examples:
- Get all users: `GET /users/`
- Add a new user: `POST /users/`

### Filtering resource collections

Resource collections are always filtered using query parameters.

Examples:
- Get all users with a specific role: `GET /users/?role=...`

### Resource detail

Resources are subdirectories of their collection, identified by an id or slug.
They have no trailing slash.

Examples:
- Get a specific user: `GET /users/{userId}`
- Create a user with a specific id: `PUT /users/{userId}`
- Update a specific user: `PATCH /users/{userId}`
- Delete a specific user: `DELETE /users/{userId}`

### Singletons

Singletons are resources of which only one can ever exist. Singletons are thus always singular.
They have no trailing slash, as they represent a single resource.

Examples:
- Get info about the current user: `GET /user`
- Update the title of an event: `PUT /events/{eventId}/title`

### POST vs PUT vs PATCH

Use `POST` requests when creating new resources in a collection, and when you don't know the resource id beforehand.
Send all data using a JSON payload.
For example: `POST /events/`, `POST /places/`, ...

Use `PUT` requests when creating/updating resources in a collection, and when you already know the resource id.
Send any additional data in a JSON payload just like a `POST` request.
For example: `PUT /events/{eventId}/labels/{labelName}`

Additionally, use `PUT` requests when making partial changes to resources. Structure the request so the property being updated acts as a singleton.
Send any additional data in a JSON payload just like a `POST` request.
For example: `PUT /events/{eventId}/title`, ...

When linking two resources to each other, use a `PUT` request to a resource in a collection OR a singleton (many vs single), on the parent resource.
For example: `PUT /events/{eventId}/labels/{labelName}` for one-to-many relations, `PUT /events/{eventId}/organizer` for one-to-one relations.

Never use `PATCH`.

### DELETE

Specify the resource id in the URL, never in a JSON payload.
For example: `DELETE /events/{eventId}`, `DELETE /events/{eventId}/labels/{labelName}`

### Casing

Endpoints should use casing that matches their use in request and response content. Do not use kebab-case to slug multiple words. Wildcards are camelcased, starting with a lowercase letter.

Example: The path to update the `typicalAgeRange` property of an event with a given `eventId`.

    PUT /events/{eventId}/typicalAgeRange
