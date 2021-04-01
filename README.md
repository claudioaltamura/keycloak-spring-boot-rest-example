[![Build Status](https://travis-ci.com/claudioaltamura/keycloak-spring-boot-rest-example.svg?branch=master)](https://travis-ci.com/github/claudioaltamura/keycloak-spring-boot-rest-example)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# keycloak-spring-boot-rest-example
A Keycloak Spring Boot REST example

## Build

    mvn clean compile

## Run

    docker-compose up -d

    mvn spring-boot:run

## Stop

    docker-compose down -v

## Examples

Security with Keycloak

UI

    http://localhost:8180/auth/

    user: admin password: Pa55w0rd

    Add realm > Import: Select file conf/keycloak-realm.jspm > Create

Get access token - simple user

    export access_token=$(\
    curl -X POST 'http://localhost:8180/auth/realms/blueprint/protocol/openid-connect/token' \
     --header 'Content-Type: application/x-www-form-urlencoded' \
     --data-urlencode 'grant_type=password' \
     --data-urlencode 'client_id=app-blueprint' \
     --data-urlencode 'client_secret=secret' \
     --data-urlencode 'username=alice' \
     --data-urlencode 'password=alice' | jq --raw-output '.access_token' \
     )

Decode the access_token using https://jwt.io.

    echo $access_token

unauthorized rest request

    curl -v http://localhost:8080/admin \

authorized rest request

    curl http://localhost:8080/user \
      -H "Authorization: Bearer $access_token"

Get access token - admin

    export access_token=$(\
    curl -X POST 'http://localhost:8180/auth/realms/myapp/protocol/openid-connect/token' \
     --header 'Content-Type: application/x-www-form-urlencoded' \
     --data-urlencode 'grant_type=password' \
     --data-urlencode 'client_id=app-myapp' \
     --data-urlencode 'client_secret=secret' \
     --data-urlencode 'username=jdoe' \
     --data-urlencode 'password=jdoe' | jq --raw-output '.access_token' \
     )

authorized rest request

    curl http://localhost:8080/admin \
      -H "Authorization: Bearer $access_token"

## Keycloak
https://www.keycloak.org/documentation
