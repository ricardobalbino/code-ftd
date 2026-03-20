# High Performance

With Dataverse being a distributed web application using the browser as the only end-user client, it is even more important carefully consider performance aspects in all areas of the architecture and in the development.

Generally, Dynamics developers must be familiar with the performance best practices.

There have been many findings which are crucial to achieve high performance in the whole application. The most prominent are:

- Avoid server roundtrips wherever possible:
- Try to bundle multiple OData requests
- Apply caching. Use cached results on forms for stable data which are only calculated once during every onload event (e.g. the process area of a team).
- Do not update IFRAME content redundantly.
- When developing web resources, use developer tools (F12) in the browser to find redundant or not cached server requests.
- Especially in the web resource area, choose to store objects and parameters in JSON notation when passing them to another component (e.g. the message bus) instead of storing them in XML to avoid costly XML traversal.
- Use full type comparison ("===" and "!==") and other techniques in Javascript to speed up client performance.
- When possible (i.e. for example not in plug-ins when dynamic variable values are needed), use static classes with helper methods instead of instantiating helper objects.
- Avoid nested loops.