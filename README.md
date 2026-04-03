# Muze Software And Applications For Solid

This documentation covers the Muze suite of libraries and nascent applications using the Solid set of protocols and standards for the decentralised web.

At the time of writing there is still much work to be done, so as yet there is little here for anyone seeking a polished suite of applications.

Instead, this project is at the stage of attracting interest from developers, so if you are a web developer then you are the primary audience.

The secondary audience of this documentation are people who are interested in Solid and the technologies which surround it, but perhaps are not going to be developing applications themselves. Here we hope that this audience will find enough information to clearly understand what is involved, without having to learn every detail which lies beneath.

## The Philosphy Behind The Muze Library Structure

The Muze javascript libraries are a collection of single purpose software components that follow a Javascript equivalent of the UNIX philosophy of small programs that do one thing, and do it well.

Each of the libraries provides a single piece of functionality, rather than there being a a large library which tries to be a Swiss Army knife of many.

Javascript libraries working with the web don’t have the luxury of UNIX’s “everything is a file” phlosophy, so they have instead to find the common boundaries such as APIs, or protocols that already exist in the web browser environment.

Muze will not cross one of those boundaries within a single library. As an example, the React nextjs framework crosses the client-server boundary by allowing Javascript seamlessly on both server and client side in the same file as though in the same environment. This causes confusion, something Muze wishes to avoid.

Thus if there is an established solution in the browser which is designed to cross one of the web’s boundaries, the libraries will use that even if it’s not the prefereed solution. This ensures that constant alignment is maintained with the direction of the web as a whole. In short, if something is built into the browser it will not be reimplemented.




