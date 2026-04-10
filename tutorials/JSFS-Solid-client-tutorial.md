# A JSFS-Solid client tutorial

JSFS-Solid is a Javascript library offering client to access Solid pods through a filesystem-like interface. In this tutorial we will take the reader through basic usage of JSFS-Solid, covering first simple access to a pod, and then working with protected resources.

## Getting Started with the JSFS-Solid Client

In this section we will cover basic set-up of a JSFS-Solid client, connect to a public pod resource, and list its contents.
    
First, include the JSFS-solid package:
```
    &lt;script src="https://cdn.jsdelivr.net/gh/muze-nl/jsfs-solid/dist/browser.js"&gt;&lt;/script&gt;
```

Then create a client
```
const client = solidClient(
    "https://auke.solidcommunity.net/",
    "/public/"
    {
        client_info: {
            client_name: "My Client"
        }
    }
}
```
 
This client allows you to connect to a Solid POD (storage server) like this:
```
const dir = await client.list()
```

Which returns something like this:
```
[
	{
		"filename": "jsontag",
		"path": "/public/jsontag/",
		"type": "folder"
	},
	{
		"filename": "data.json",
		"path": "/public/data.json",
		"type": "file"
	},
	{
		"filename": "slides-test",
		"path": "/public/slides-test/",
		"type": "folder"
	},
	{
		"filename": "img",
		"path": "/public/img/",
		"type": "folder"
	},
	{
		"filename": "solid-muze",
		"path": "/public/solid-muze/",
		"type": "folder"
	},
	{
		"filename": "SimplyStore",
		"path": "/public/SimplyStore/",
		"type": "folder"
	},
	{
		"filename": "SimplyStore%20-%20One%20Year%20Later",
		"path": "/public/SimplyStore%20-%20One%20Year%20Later/",
		"type": "folder"
	},
	{
		"filename": "nulldata.json",
		"path": "/public/nulldata.json",
		"type": "file"
	},
	{
		"filename": "slides.html",
		"path": "/public/slides.html",
		"type": "file"
	}
]
```

Filesystem-like unctions available in the client are:
    
- cd
- list
- exists
- read
- write
- delete
  

## Fetching webid and OIDC issuer with solidBase

The webID is a Linked Data resource that contains the user's profile. A core tenet of Solid is that identity and storage do not have to come from the same place, for example a single WebID can be associated with multiple pods from different providers. Many providers will store a user's WebID in their pod, but this does not have to be the case, and Muze puts WebIDs in separate storage reserved only for profiles.

The information stored in a user's WebID includes their issuer, which is a URL through which they cna be authenticated, usually at their OpenID identity provider. In this section we will retrieve the user's WebID, and extract the issuer from it.

First, we include the solidClient:
```
&lt;script src="https://cdn.jsdelivr.net/npm/@muze-nl/jsfs-solid/dist/browser.js"&gt;&lt;/script&gt;
```

The WebID should always be public, so we don't need the complexity of an OIDC client. Lets build the most basic webid client possible to fetch that profile:
```
      const webidClient = metro.client().with(oldmmw())
      const profile = await webidClient.get("https://auke.solidcommunity.net/profile/card#me")
      const issuer = profile.data.primary.solid$oidcIssuer
```
Which returns something like this:

```
{
	"id": "https://solidcommunity.net/"
}
```

`metro` and `oldmmw` are included in the solidClient script.

## Fetching OIDC issuer with metro.api

In the previous step, we retrieved the OpenID issuer from the WebID. We will now call the OpenID issuer.

First, include the solidClient:

```
&lt;script src="https://cdn.jsdelivr.net/npm/@muze-nl/jsfs-solid/dist/browser.js"&gt;&lt;/script&gt;
```

Now lets build a slightly more advanced webid api, that will throw an Error if there is a network issue.
```
const webidClient = metro.api(
  metro.client().with(oldmmw()),
  {
    issuer: async function(url) {
      const profile = await this.get(url)
      return profile.primary?.solid$oidcIssuer?.id
    }
  }
)
```

Its only custom api method is `issuer(url)`, which fetches a webid url, parses it and returns the issuer url.

```
const issuer = await webidClient.issuer("https://auke.solidcommunity.net/profile/card#me")
```

Which returns something like this:
```
https://solidcommunity.net/
```

All `metro.api` clients will also support the normal http fetch methods, like `get`, `post`, etc.

