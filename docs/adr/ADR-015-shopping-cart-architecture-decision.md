# ADR-015: Shopping Cart Architecture Decision

- **Status**: Proposed
- **Date**: 2025-09-25

## Context

The e-commerce platform needs a shopping cart feature that supports adding, updating, and removing items, and persists across user sessions. The application is built with a stateless microservice architecture and requires high availability.

## Decision

Implement the shopping cart as a separate microservice that stores cart state in Redis. The service exposes REST endpoints for cart operations and uses Redis as the primary data store. Cart data is keyed by user session ID and includes item IDs, quantities, and timestamps. The service will handle Redis connection failures gracefully and provide fallback mechanisms.

## Consequences

Storing cart data in Redis allows stateless application instances and easy horizontal scaling. It introduces a dependency on Redis and requires handling Redis failures. It also means cart data is lost if Redis is not persisted or if the user logs out before checkout.

## Architecture Diagram

A simple flow diagram showing user requests to the cart service, Redis interactions, and eventual checkout process.