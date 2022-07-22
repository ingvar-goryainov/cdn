# Handmade CDN

## What is a CDN?

A Content Delivery Network is a set of computers, spatially distributed in order to provide high availability and **better performance** for systems that have their **work cached** on this network.

## Why do you need a CDN?

A CDN helps to improve:
* loading times (smoother streaming, instant page to buy, quick friends feed, etc)
* accommodate traffic spikes (black friday, popular streaming release, breaking news, etc)
* decrease costs (traffic offloading)
* scalability for millions

## How does a CDN work?

CDNs are able to make services faster by placing the content (media files, pages, games, javascript, a json response, etc) closer to the users.

When a user wants to consume a service, the CDN routing system will deliver the "best" node where the content is likely **already cached and closer to the client**.

# Origin - the backend service

Origin is the system where the content is created - or at least it's the source to the CDN. The sample service we're going to build will be a straightforward JSON API. The backend service could be returning an image, video, javascript, HTML page, game, or anything you want to deliver to your clients.

We'll use Nginx and Lua to design the backend service.

### Test least connected load balancing

Let's add `least_conn;` configuration to `upstream {}` block and reload nginx in lb container to update config.

We don't have static sessions in our test environment and we have the same result as with previous configuration.


### Test session persistence load balancing

Let's add `ip_hash;` configuration to `upstream {}` block and reload nginx in `lb` container to update config and test.

### Pros and cons of each approach (some point were found [here](https://www.nginx.com/blog/choosing-nginx-plus-load-balancing-techniques/#Pros-Cons-and-Use-Cases))

#### Default load balancing
+ Easy setup, no additional configuration.
- Bad solution for dynamic content/application servers.

#### Least connected load balancing
+ Nginx will try not to overload a busy application server with excessive requests, distributing the new requests to a less busy server instead.
- With each connection you can receive content from a different server. It's also bad for dynamic content.

#### Session persistence load balancing
+ Requests from the same client will always be directed to the same server except when this server is unavailable.
- The biggest drawback of these methods is that they are not guaranteed to distribute requests in equal numbers across servers, let alone balance load evenly. 
