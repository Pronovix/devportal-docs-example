# Documentation

API documentation describes what services an API offers and how to use those services, aiming to cover everything a client would need to know for practical purposes.

Documentation is crucial for the development and maintenance of applications using the API.
API documentation is traditionally found in documentation files but can also be found in social media such as blogs, forums, and Q&A websites.

Traditional documentation files are often presented via a documentation system, such as [Markdown](https://en.wikipedia.org/wiki/Markdown "Link to Wikipedia").

The types of content included in the documentation differs from API to API.

In the interest of clarity, API documentation may include a description of classes and methods in the API as well as typical usage scenarios, code snippets and design rationales.

Restrictions and limitations on how the API can be used are also covered by the documentation.

For instance, documentation for an API function could note that its parameters cannot be null, that the function itself is not thread safe,or that a decrement and cancel protocol averts self-trading.

Because API documentation tends to be comprehensive, it is a challenge for writers to keep the documentation updated and for users to read it carefully, potentially yielding bugs.

API documentation can be enriched with metadata information like Java annotations.
This metadata can be used by the compiler, tools, and by the run-time environment to implement custom behaviors or custom handling.

It is possible to generate API documentation in data-driven manner.

By observing a large number of programs that use a given API, it is possible to infer the typical usages, as well the required contracts and directives.

Then, templates can be used to generate natural language from the mined data.
