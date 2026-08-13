# ![RealWorld Example App using Kotlin and Spring](example-logo.png)

[![Actions](https://github.com/gothinkster/spring-boot-realworld-example-app/workflows/Java%20CI/badge.svg)](https://github.com/gothinkster/spring-boot-realworld-example-app/actions)

> ### Spring boot + MyBatis codebase containing real world examples (CRUD, auth, advanced patterns, etc) that adheres to the [RealWorld](https://github.com/gothinkster/realworld-example-apps) spec and API.

This codebase was created to demonstrate a fully fledged full-stack application built with Spring boot + Mybatis including CRUD operations, authentication, routing, pagination, and more.

For more information on how to this works with other frontends/backends, head over to the [RealWorld](https://github.com/gothinkster/realworld) repo.

# *NEW* GraphQL Support  

Following some DDD principles. REST or GraphQL is just a kind of adapter. And the domain layer will be consistent all the time. So this repository implement GraphQL and REST at the same time.

The GraphQL schema is https://github.com/gothinkster/spring-boot-realworld-example-app/blob/master/src/main/resources/schema/schema.graphqls and the visualization looks like below.

![](graphql-schema.png)

And this implementation is using [dgs-framework](https://github.com/Netflix/dgs-framework) which is a quite new java graphql server framework.
# How it works

The application uses Spring Boot (Web, Mybatis).

* Use the idea of Domain Driven Design to separate the business term and infrastructure term.
* Use MyBatis to implement the [Data Mapper](https://martinfowler.com/eaaCatalog/dataMapper.html) pattern for persistence.
* Use [CQRS](https://martinfowler.com/bliki/CQRS.html) pattern to separate the read model and write model.

And the code is organized as this:

1. `api` is the web layer implemented by Spring MVC
2. `core` is the business model including entities and services
3. `application` is the high-level services for querying the data transfer objects
4. `infrastructure`  contains all the implementation classes as the technique details

# Security

Integration with Spring Security and add other filter for jwt token process.

The secret key is stored in `application.properties`.

# Database

It uses a ~~H2 in-memory database~~ sqlite database (for easy local test without losing test data after every restart), can be changed easily in the `application.properties` for any other database.

# API Endpoints

The application provides a REST API following the [RealWorld API spec](https://github.com/gothinkster/realworld/tree/master/api). All endpoints are accessible at `http://localhost:8080` (not `/api`).

In addition to REST, the application exposes a GraphQL API (Netflix DGS): `POST /graphql` (the endpoint is publicly reachable; read queries work unauthenticated, while article/comment/follow mutations require a JWT) and `GET /graphiql` (interactive playground).

## Authentication

Authentication is handled via JWT tokens. Include the token in the `Authorization` header:
```
Authorization: Token {jwt-token}
```

## User & Authentication

- `POST /users` - Register a new user
- `POST /users/login` - Login existing user
- `GET /user` - Get current user (requires auth)
- `PUT /user` - Update current user (requires auth)

## Profiles

- `GET /profiles/:username` - Get user profile
- `POST /profiles/:username/follow` - Follow a user (requires auth)
- `DELETE /profiles/:username/follow` - Unfollow a user (requires auth)

## Articles

- `GET /articles` - List articles (supports query params: `tag`, `author`, `favorited`, `limit`, `offset`)
- `GET /articles/feed` - Get user's article feed (requires auth)
- `POST /articles` - Create article (requires auth)
- `GET /articles/:slug` - Get article by slug
- `PUT /articles/:slug` - Update article (requires auth)
- `DELETE /articles/:slug` - Delete article (requires auth)

## Favorites

- `POST /articles/:slug/favorite` - Favorite an article (requires auth)
- `DELETE /articles/:slug/favorite` - Unfavorite an article (requires auth)

## Comments

- `GET /articles/:slug/comments` - Get comments for article
- `POST /articles/:slug/comments` - Add comment to article (requires auth)
- `DELETE /articles/:slug/comments/:id` - Delete comment (requires auth)

## Tags

- `GET /tags` - Get all tags

# Getting started

## Prerequisites

- Java 11–17 (the pinned Gradle 7.4 wrapper supports JDK up to 17)
- Gradle (wrapper included)

## Running locally

Start the application:

    ./gradlew bootRun

The server will start on port 8080. To verify it's working:

    curl http://localhost:8080/tags

Or open http://localhost:8080/tags in your browser.

## Configuration

The application uses SQLite for local development. Configuration is in `src/main/resources/application.properties`:

- **Database**: `dev.db` (SQLite file created automatically)
- **JWT Secret**: Configured for development (change for production)
- **JWT Session Time**: 86400 seconds (24 hours)
- **Server Port**: 8080 (default)

No additional database setup is required - the SQLite database is created automatically on first run.

# Try it out with [Docker](https://www.docker.com/)

You'll need Docker installed.
	
    ./gradlew bootBuildImage --imageName spring-boot-realworld-example-app
    docker run -p 8081:8080 spring-boot-realworld-example-app

# Try it out with a RealWorld frontend

The entry point address of the backend API is at http://localhost:8080, **not** http://localhost:8080/api as some of the frontend documentation suggests.

# Run test

The repository contains a lot of test cases to cover both api test and repository test.

    ./gradlew test

# Code format

Use spotless for code format.

    ./gradlew spotlessJavaApply

# Help

Please fork and PR to improve the project.
