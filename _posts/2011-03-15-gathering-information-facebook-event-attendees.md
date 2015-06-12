---
layout: post
title:  "Gathering information about your Facebook-event-attendees"
date:   2011-03-15
categories:
- facebook
- graph-api
---

We're currently in the planning phase for our next "Vorabifeier" (we're
celebrating our future Abitur) and we created a Facebook-event-page to keep
track of the expected guests. The reactions were huge, so we thought "Hey,
couldn't we use this system to collect some information about the attendees?".

With Facebook, the data kraken no. 1, this shouldn't be such a big problem.
Indeed, there's this nice Graph API, so I started hacking a tiny Python-script
which first gets the list of the people attending our event, then gathers some
information about each of this persons and finally does some crazy stuff with
it (well, it could do some crazy stuff, in this example it only counts the
people of full age).

Maybe you could make use of it.

So, here it is:

{% gist lukasklein/f9c4ef83c1c9ddf44d6f %}
