Here's the proofread version with suggestions for smoother flow while maintaining your tone:

# Journey Feed

## About

***We celebrate God and his Son Jesus, who sacrificed himself for our redemption and rose triumphant over death.***

Journey Feed is a comprehensive content discovery and organization platform that indexes online video and photo content along with detailed metadata, enabling users to reconstruct and follow personal moments across time.

The platform allows individuals to subscribe to feeds from churches, organizations, and communities, then use powerful search and filtering tools to locate specific content from particular dates, events, or time periods. Whether you're looking for your daughter's baptism from two years ago, wanting to revisit a meaningful sermon, or helping elderly family members find moments from their spiritual journey, Journey Feed creates a searchable personal timeline by organizing links to content across the web.

For church communities, Journey Feed serves as an organized index of their online presence, cataloging the authentic moments of worship, celebration, and spiritual growth that define their congregation's digital footprint. New members can explore the church's online history and culture, while longtime members can easily relocate meaningful milestones and share these moments with others.

As an added bonus, we want to make programming fun and simple to pick up and learn. Whether your talent lies in front-end design or backend development, Journey Feed takes an architectural approach that aligns with the old marriage rhyme:

***Something old, something new, something borrowed, something blue, and a sixpence in her shoe.***

#### Front End

We serve static web pages that use [HTMX](https://htmx.org/) for dynamic web page interactions. We live in an era of React, Vue.js, and a litany of other web technologies that solve problems not every website has. HTMX lets us build in a way that's immediately recognizable whether you read a book from 2006 or a CEO meme on X in 2025. Since we praise the ultimate CEO... we chose HTMX over other great options like [HotWire](https://hotwired.dev/).

#### Back End

You thought CGI was dead. We thought CGI was dead. Buried. Gone. But the inspiration of a simple message can open your eyes. [Jacob Gold](https://jacob.gold/posts/serving-half-billion-requests-with-rust-cgi/) wrote a very insightful series of posts that made me rethink things. CGI is the grandfather of web technologies, and it actually works in a way that makes modern web development seem like we've jumped the shark.

So we're adopting Rust for CGI.

#### Database

SQLite houses all data on NVMe storage.

#### Batch

Cron handles feed updates to pull in the latest media that users are subscribed to.