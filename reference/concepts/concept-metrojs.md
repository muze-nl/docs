# Concept: MetroJS
 
MetroJS is one of the Muze javascript libraries. It provides an HTTPS client with support for middleware.

(This page could use a diagram)

It is designed to be compatible with the Fetch API, while acting as a proxy between your code and the real Fetch API. This proxying arrangement allows for middleware, other software which can act upon the requests. For example middleware might be used to handle authentication, or to process the retrieved data into a consistent format.
