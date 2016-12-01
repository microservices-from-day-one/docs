# Microservices From Day One

This is a repository with documentation that applies to the bookstore project, each application/service and client libraries lives in their own repository.

## Description of folders/services

### Services (port, name, description)

* 5000 - [books](https://github.com/microservices-from-day-one/books) - books services
* 5001 - [purchases](https://github.com/microservices-from-day-one/purchases) - purchases services
* 5002 - [users](https://github.com/microservices-from-day-one/users) - accounts service
* 5004 - [showcase](https://github.com/microservices-from-day-one/showcase) - user-facing web interface
* 5005 - [reviews](https://github.com/microservices-from-day-one/reviews) - reviews service

### Libraries / other

* [api-client](https://github.com/microservices-from-day-one/api-client) - Ruby single api client for all services
* [api-testing](https://github.com/microservices-from-day-one/api-testing) - Cucumber based integration test suite
* [service_discovery](https://github.com/microservices-from-day-one/service_discovery) - Ruby service discovery/consul wrapper

## Software

### Ruby

We use [ruby](https://www.ruby-lang.org/en/) in `purchases`, `users`, `reviews`, and `showcase`, we've developed all Ruby code against that version `2.3.1`. You can install ruby following instructions at https://www.ruby-lang.org/en/downloads/.

### Rust

We use [rust](https://www.rust-lang.org/) in `books`, we've verified that version `1.13` can be used to compile and run the service, it's very likely that the codebase will work as expected with all `1.x` versions. You can install rust following instructions at https://www.rust-lang.org/en-US/downloads.html.

```
22:08 $ rustc --version
rustc 1.13.0 (2c6933acc 2016-11-07)
```

### PostgreSQL

All services have been developed with a PostgreSQL database; therefore, that is a requirement to run those services. PostgreSQL can be downloaded at https://www.postgresql.org/download/.

### Service Discovery / Setup

We are experiencing with consul - http://consul.io - as a tool for service discovery, current version is `0.7.1`. It's mainly used to hold data about service hosts for now. Ideally, we'd be able to switch service host/ports between on-the-cloud and local running versions of each service, having that reflected in all applications.

To start using consul, one needs to [install it](https://www.consul.io/intro/getting-started/install.html), and start it in dev mode with:

`consul agent -dev -config-dir=/etc/consul.d`

Ideally, we'd write a script to bootstrap consul with known services/ports, either running locally or in a cloud-based development environment. For now, use add all services to consul via:

```
echo '{"service": {"name": "books", "tags": ["rust"], "port": 5000}}' | sudo tee /etc/consul.d/books.json
echo '{"service": {"name": "purchases", "tags": ["rails"], "port": 5001}}' | sudo tee /etc/consul.d/purchases.json
echo '{"service": {"name": "users", "tags": ["rails"], "port": 5002}}' | sudo tee /etc/consul.d/users.json
echo '{"service": {"name": "reviews", "tags": ["rails"], "port": 5005}}' | sudo tee /etc/consul.d/reviews.json
```
