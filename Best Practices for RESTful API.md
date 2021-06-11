# Best Practices For Designing a Pragmatic RESTful API



### Key Requirements for the API

- It should use web standards where they *make sense*
- It should be friendly to the developer and be explorable via a browser address bar
- It should be simple, intuitive and consistent to make adoption not only easy but pleasant
- It should provide enough flexibility to power majority of the [Enchant](http://www.enchant.com/) UI
- It should be efficient, while maintaining balance with the other requirements

An API is a developer's UI - just like any UI, it's important to ensure the user's experience is thought out carefully!

---

### How to properly make a resource ?

Step 1:

- Should be ` nouns (objects)` that make sense from the perspective of the API consumer, not verbs. Example: Ticket, Customer, User
- The key is not to leak irrelevant implementation details out to your API. API resources need to make sense from the perspective of the API consumer.

Step 2:

- Identify the actions that apply to them and how it would map to the API. 

  Example:

  - GET /tickets - Retrieves a list of tickets
  - GET /tickets/12 - Retrieves a specific ticket
  - POST /tickets - Creates a new ticket
  - PUT /tickets/12 - Updates ticket #12
  - PATCH /tickets/12 - Partially updates ticket #12
  - DELETE /tickets/12 - Deletes ticket #12



** Note: ** Always keep the URL format consistent and always use a plural. 

**But how do you deal with relations?** If a relation can only exist within another resource, RESTful principles provide useful guidance. Let's look at this with an example. A ticket in [Enchant](http://www.enchant.com/) consists of a number of messages. These messages can be logically mapped to the /tickets endpoint as follows:

- GET /tickets/12/messages - Retrieves list of messages for ticket #12
- GET /tickets/12/messages/5 - Retrieves message #5 for ticket #12
- POST /tickets/12/messages - Creates a new message in ticket #12
- PUT /tickets/12/messages/5 - Updates message #5 for ticket #12
- PATCH /tickets/12/messages/5 - Partially updates message #5 for ticket #12
- DELETE /tickets/12/messages/5 - Deletes message #5 for ticket #12

**Alternative 1**: If a relation can exist independently of the resource, it makes sense to just include an identifier for it within the output representation of the resource. The API consumer would then have to hit the relation's endpoint.

**Alternative 2**: If an independently existing relation is commonly requested alongside the resource, then the API could offer functionality to [automatically embed](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#autoloading) the relation's representation and avoid the second hit to the API. Clean API and one hit to the server. I like this approach.

**What about actions that don't fit into the world of CRUD operations?**

This is where things can get fuzzy. There are a number of approaches:

1. Restructure the action to appear like a field of a resource. This works if the action doesn't take parameters. For example an *activate* action could be mapped to a boolean activated field and updated via a PATCH to the resource.
2. Treat it like a sub-resource with RESTful principles. For example, GitHub's API lets you [star a gist](http://developer.github.com/v3/gists/#star-a-gist) with PUT /gists/:id/star and [unstar](http://developer.github.com/v3/gists/#unstar-a-gist) with DELETE /gists/:id/star.
3. Sometimes you really have no way to map the action to a sensible RESTful structure. For example, a multi-resource search doesn't really make sense to be applied to a specific resource's endpoint. In this case, /search would make the most sense even though it isn't a resource. This is OK - just do what's right from the perspective of the API consumer and make sure it's documented clearly to avoid confusion.

---

## Updates & creation should return a resource representation

A PUT, POST or PATCH call may make modifications to fields of the underlying resource that weren't part of the provided parameters (for example: created_at or updated_at timestamps). To prevent an API consumer from having to hit the API again for an updated representation, have the API return the updated (or created) representation as part of the response.

In case of a POST that resulted in a creation, use a [HTTP 201 status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.5) and include a [Location header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30) that points to the URL of the new resource. Both of those in should be in addition to including the newly created resource representation as the body of the response.