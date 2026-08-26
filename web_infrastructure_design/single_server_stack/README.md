# Single Server Web Stack

This project models a simple web application hosted on a single server and reached through `www.foobar.com`.

## Architecture

The request path is represented in [`single_server_stack.mmd`](single_server_stack.mmd).

The user accesses `www.foobar.com`. DNS resolves the domain to the server IP address `8.8.8.8` using an A record. The request then reaches the Nginx web server, which passes it to the application server and application code before accessing the database. The response follows the reverse path back through the application layers and Nginx to the user.

## Server

A server is a physical or virtual computer that provides resources and services to other systems over a network. It runs an operating system that manages its hardware or virtual resources and provides the environment required to run applications.

Servers are commonly hosted in data centers, where they can be connected to reliable power, cooling, storage, and network infrastructure. The server itself is different from the services running on it: Nginx, the application server, application code, and the database are software components hosted by the server.

## Domain Name and DNS

`www.foobar.com` is a domain name used by the user to reach the website without having to remember an IP address.

DNS (Domain Name System) translates domain names into IP addresses. In this scenario, the `www` record is an A record because it maps the hostname directly to an IPv4 address: `8.8.8.8`.

The A record therefore allows the user's request for `www.foobar.com` to be directed to the single server hosting the web application.

## Web Server

The web server is Nginx. It receives HTTP requests from the user and acts as the entry point to the application.

Nginx forwards requests to the application server and sends the application's response back to the user.

## Application Server

The application server runs the server-side application and handles the logic required to process requests.

It receives requests from Nginx, executes the required application code, and communicates with the database when data is needed.

## Application Code

The application code contains the business logic of the website.

It processes the user's request, determines what actions are required, retrieves or modifies data through the database, and produces the response that is returned to the user.

## Database

The database stores and manages the application's persistent data.

In this architecture, the database is PostgreSQL or MySQL. The application code communicates with the database to retrieve, create, update, or delete the data required to process requests.

## Network Communication

The user and the server communicate across a network using the TCP/IP protocol suite.

TCP provides reliable communication between the client and server, while IP is responsible for addressing and routing packets between networks.

## LAMP and Nginx

LAMP stands for:

- **Linux** - operating system
- **Apache** - web server
- **MySQL** - database
- **PHP** - programming language

This architecture is LAMP-like because it follows the same general idea of combining an operating system, web server, application layer, and database. However, it is not a literal LAMP stack because it uses **Nginx instead of Apache**, and the application technology is not necessarily PHP.

A more accurate description would therefore be an Nginx-based web stack.

## Single Point of Failure

The entire architecture depends on a single server. This makes the server a **single point of failure**.

If the server becomes unavailable, the web server, application server, application code, and database are all unavailable, causing the website to go down.

## Maintenance

Maintenance on the single server can cause downtime because all services are hosted on that same server.

For example, restarting or shutting down the server to perform system maintenance would make the entire application unavailable until the server is operational again.

## Capacity and Traffic

The single server also has a limited capacity.

As traffic increases, the server's CPU, memory, storage, and network resources can become saturated. Once the server reaches its capacity, it may respond more slowly or become unable to handle additional requests.

This architecture therefore does not provide redundancy or horizontal scaling and is best suited to a simple application with limited traffic.
