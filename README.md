# jev-spring-decision

A small Spring Boot 4 app that calls the [TypeSafe Jev API](https://docs.typesafe.ai/api) with Spring's `RestClient`.

Jev answers typed questions about a piece of content. You send some state (a string, an object, or an array) plus a map of questions. Each question is one of three types:

| Type | Ask | You get back |
| --- | --- | --- |
| `noul` | A yes or no question | A probability from 0 to 1 |
| `choice` | Pick one option from a set | The choice, a probability per option, and a confidence |
| `score` | Rate against ordered levels | A weighted score, a legend, probabilities, and a confidence |

## Requirements

- JDK 25 or later (check with `java --version`)
- A TypeSafe API key from <https://console.typesafe.ai/settings/keys>

## How it works

- `JevClient` builds a `RestClient` from the auto-configured builder, sets the base URL and the bearer token, and posts to `/v1/systemone`.
- `Question` is a record with static factories for `noul`, `choice`, and `score`. Criteria are left out of the JSON when they are null.
- `JevResponse` maps the answers back into records so you work with typed fields instead of raw JSON.
- `JevDemo` is a `CommandLineRunner` that builds the sample request and prints the results.
- `JevClientTest` uses `@RestClientTest` and `MockRestServiceServer` to check the request JSON and parse a documented response without calling the real API.

