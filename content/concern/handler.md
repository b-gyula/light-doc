---
title: "Handler"
date: 2017-11-05T10:24:06-05:00
description: ""
categories: [concerns]
keywords: []
menu:
  docs:
    parent: "concern"
    weight: 55
weight: 55	#rem
aliases: []
toc: false
draft: false
---


A interface for middleware handlers. All middleware handlers must implement this interface
so that the handler can be plugged in to the request/response chain during server startup
with SPI (Service Provider Interface). The entire light-4j framework is a core server that
provides a plugin structure to hookup all sorts of plugins to handler different cross-cutting
concerns.

The middleware handlers are designed based on chain of responsibility pattern.

## Interface
```
public interface MiddlewareHandler extends HttpHandler {

    /**
     * Get the next handler in the chain
     *
     * @return HttpHandler
     */
    HttpHandler getNext();

    /**
     * Set the next handler in the chain
     *
     * @param next HttpHandler
     * @return MiddlewareHandler
     */
    MiddlewareHandler setNext(final HttpHandler next);

    /**
     * Indicate if this handler is enabled or not.
     *
     * @return
     */
    boolean isEnabled();

    /**
     * Register this handler to the handler registration.
     */
    void register();

}

```

## Implementations

There are example of implementations in [middleware][]

[middleware]: /concern/
