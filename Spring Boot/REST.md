REST Specifications:

REpresentational State Transfer

the key abstraction of information in REST is a resource.

&nbsp;

&nbsp;

Any information that can be named can be a resource: a document or image, a temporal service, a collection of other resources, a non-virtual object (e.g. a person), and so on.

&nbsp;

&nbsp;

REST uses a **resource identifier** to identify the particular resource involved in an interaction between components.

&nbsp;

The state of the resource at any particular timestamp is known as resource representation.

&nbsp;

A representation consists of data, metadata describing the data and hypermedia links which can help the clients in transition to the next desired state.

&nbsp;

**Hypertext (or hypermedia) means the simultaneous presentation of information and controls such that the information becomes the affordance through which the user (or automaton) obtains choices and selects actions. Hypertext can be anything xml,json,etc.**

With HATEOAS, a client interacts with a network application whose application servers provide information dynamically through hypermedia.

the client does not need to know if a resource is employee or device. It should act on the basis of media-type associated with the resource.

i.e. resource representations shall be self-descriptive.

resource methods are to be used to perform the desired transition.

Guiding Principle:

1.Client–server :req is always originated from client

2.Stateless– Each request from client to server must contain all of the information necessary to understand the request,Session state is therefore kept entirely on the client.

3.Cacheable :

\*\*4.Uniform interface: \*\*