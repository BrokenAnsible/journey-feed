Here's the proofread version with suggestions for smoother flow while maintaining your tone:

# Journey Feed

## About

***We celebrate God and his Son Jesus, who sacrificed himself for our redemption and rose triumphant over death.***

Journey Feed is software designed to help people find and subscribe to the message of Jesus Christ being preached around the world online.

Moreover, we want to make programming fun and simple to pick up and learn. Whether your talent lies in front-end design or backend development, Journey Feed takes an architectural approach that aligns with the old marriage rhyme:

***Something old, something new, something borrowed, something blue, and a sixpence in her shoe.***

#### Front End

We serve static web pages that use [HTMX](https://htmx.org/) for dynamic web page interactions. We live in an era of React, Vue.js, and a litany of other web technologies that solve problems not every website has. HTMX lets us build in a way that's immediately recognizable whether you read a book from 2006 or a CEO meme on X in 2025. Since we praise the ultimate CEO... we chose HTMX over other great options like [HotWire](https://hotwired.dev/).

#### Back End

You thought CGI was dead. We thought CGI was dead. Buried. Gone. But the inspiration of a simple message can open your eyes. [Jacob Gold](https://jacob.gold/posts/serving-half-billion-requests-with-rust-cgi/) wrote a very insightful series of posts that made me rethink things. CGI is the grandfather of web technologies, and it actually works in a way that makes modern web development seem like we've jumped the shark.

So we're adopting Rust for CGI.

We're bold and crazy, so we're not building admin screens for site administrators in CGI. Nope. We're implementing site and feed management using [Charm Wish](https://charm.sh/), which is based on Go.

Yes, both Rust and Go will handle backend concerns. With this setup, we should be able to serve many requests on minimal hardware. So if we want to offer a dedicated instance of Journey Feed or others want to host their own, it will scale up and down based on user count. The promise of serverless, without the lies.

#### Database

SQLite houses all data on NVMe storage.

#### Batch

Cron handles feed updates to pull in the latest media that users are subscribed to.