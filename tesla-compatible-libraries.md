# Tesla-compatible libraries

## Background

When implementing libraries which have to sent HTTP/HTTPS requests, we have to implement an HTTP client.

In most of time, people implements their own HTTP client with their preferred underlying library, such as: `httpc`, `hackney`, `mint`, `finch`, etc.

But, maybe that's not the good way.

Doing so not only wastes the time but also makes the libraries inflexible. If using multiple libraries, and these libraries use different HTTP clients, we will have many different implementations of HTTP clients running at the same time.

What's the big deal?

Using multiple implementations of HTTP clients in one project, not only consumes more resources but may also creates inconsistencies in usage patterns, complicates maintenance, observation and debugging, etc.

It would be good if we can have a unified HTTP implementation, and then each library author can use it. In the way:

- every library author can add multiple HTTP-adapters support in short time.
- every library user can choose their preferred HTTP client in a flexible way.

## What to use?

After investigating existing HTTP implementations, I found [elixir-tesla/tesla](https://github.com/elixir-tesla/tesla), which considers what I said above.

Tesla provides a very flexible way to configure adapters. Let's say we want use hackney as the adapter.

1. Set an adapter at module level in the module itself:

```elixir
defmodule GitHubClient do
  use Tesla

  adapter Tesla.Adapter.Hackney, recv_timeout: 30_000

  # ...
end
```

2. Set an adapter at module level in the config file:

```elixir
config :tesla, GitHubClient, adapter: {Tesla.Adapter.Hackney, [recv_timeout: 30_000]}
```

3. Set an adapter at app level in the config file (this is not supported for now, but I have created an [PR](https://github.com/elixir-tesla/tesla/pull/640) for it):

```
config :tesla, :my_app, adapter: {Tesla.Adapter.Hackney, [recv_timeout: 30_000]}
```

4. Set an adapter at global level in the config file:

```elixir
config :tesla, adapter: {Tesla.Adapter.Hackney, [recv_timeout: 30_000]}
```

## Last

Everyone can benefit from using Tesla:

- every library author can add multiple HTTP-adapters support in short time.
- every library user can choose their preferred HTTP client in a flexible way.

Consider using it when writing your next package. If you do, remember to include a badge in your `README.md`, like:

[![Tesla compatible](https://img.shields.io/badge/Tesla%20compatible-6e4a7e?Color=white)](https://github.com/elixir-tesla/tesla)

```
[![Tesla compatible](https://img.shields.io/badge/Tesla%20compatible-6e4a7e?Color=white)](https://github.com/elixir-tesla/tesla)
```

That's it. ;)
