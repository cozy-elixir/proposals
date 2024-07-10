# Ship utilities for building platform SDKs

> This is an evolution of [my previous proposal](./tesla-compatible-libraries.md).

After writing multiple platform SDKs for myself and my employers, I found two main problems:

1. including an HTTP client in the SDKs is very inflexible.
2. trying to implement all the APIs provided by a platform is very uneconomical.

## For problem 1

Today, most platforms provide APIs built on top of "HTTP + specific authentication methods." Therefore, building SDKs for them basically equals to "building and sending HTTP requests".

### Building HTTP requests

Everyone has their preferences for defining interfaces. If SDK users' preferences differ from the maintainer, they will find the process of using the SDK very awkward.

### Sending HTTP requests

Everyone also has their preferences for HTTP clients. Some prefer higher-level abstractions like Tesla, while others prefer lower-level implementations like Finch.

And, if a project includes multiple SDKs that each come with their own HTTP client, multiple HTTP clients will coexist in the project, which is a disaster for maintainence.

> Using multiple HTTP clients in one project, not only consumes more resources but may also creates inconsistencies in usage patterns, complicates maintenance, observation and debugging, etc.

## For problem 2

From the SDK maintainers' perspective, SDKs need to handle almost all usage scenarios, leading to high development and maintenance cost.

From the SDK users' perspective, SDKs often try to hide details, adding many extra abstractions. This increases usage cost, as users must understand both the platform's concepts and the additional abstractions from the SDK. If any issue arises, there are extra debugging cost.

## More problems

### Timeliness

Platforms are constantly updated. It's challenging to maintain individual SDKs and ensure they remain timely.

From the SDK maintainers' perspective, they have to catch up latest updates. This can easily lead to burnout.

From the SDK users' perspective, they have to wait for using the latest features which have been provided by the platform, if the maintainer is not actively maintaining the SDK. Of course, they can fork, but now they need to read new code and maintain more code.

### Documentation

While most SDKs include documentation, it rarely rivals the official documentation in quality, such as guides, examples, extra context, etc.

### Performance

SDKs often encompass all functionalities of a platform, leading to the introduction of a substantial amount of code.

## The solution

Release SDK utilities that only provide essential functionalities which help users to build an SDK, such as:

- building authentication header
- ...

And, leaving all other decisions to the users:

- what to implement in the SDK (all the APIs or just what they need)
- how to get and use configurations
- how to build and send requests
- how to handle errors
- how to test
- ...

In this way:

- The maintainers won't spend much time on maintenance anymore, as there is considerably less code to maintain.
- The users can implement their own SDKs at a lower cost, as the hardest parts have been implemented by the SDK utilities. And, they will have complete freedom.

## Packages

Pakcages I built for this paradigm:

- [http_spec](https://github.com/cozy-elixir/http_spec)

## References

- [SDKs with Req: Stripe](https://dashbit.co/blog/sdks-with-req-stripe)
