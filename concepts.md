# Concepts

In this section, we'll explore the core concepts of Light Services, which include **Arguments**, **Steps**, **Outputs**, **Context**, and **Errors**.

## Arguments

Arguments are the inputs provided to a service when it is invoked. They can be validated by type, assigned default values, and be designated as optional or required.

[Read more about arguments](arguments.md)

## Steps

Steps are the fundamental units of work within a service, representing each individual task a service performs. They can be executed conditionally, retried (this feature is currently in development), or skipped.

[Read more about steps](steps.md)

## Outputs

Outputs are the results produced by a service upon its completion. They can have default values and be validated by type.

[Read more about outputs](outputs.md)

## Context

Context refers to the shared state that passes between services in a service chain, enabling the transfer of arguments and error states from one service to another.

[Read more about context](context.md)

## Errors

Errors are exceptions that occur during the execution of a service. When an error occurs, the execution halts, and all services within the same context chain stop as well. Each service is wrapped in a database transaction; if an error arises, the transaction is rolled back, although this can be disabled if necessary.

[Read more about errors](errors.md)

