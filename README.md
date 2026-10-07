# Completion Caching

This is an experiment in getting AI to drive forward an idea I had into a manuscript.

I wrote out some ideas around how LLM chat providers could cache key-value pairs (keys are prefill embeddings of prompts, values are completions) 
to user prompts and retrieve completions based on prompt prefill similarity, perhaps modifying parts of the completion based on 
dynamical information. The idea is to route certain prompts away from decode loops, since they are much more expensive than prefill;
if one can get away with a prefill + embedding lookup + completion retrieval, that would save a lot on FLOPs, latency, and $$.

I think it is most relevant to LLM-powered search providers like Google / Google DeepMind, who probably get many repeat queries.

In some cases the completions may be fully reusable, if they do not require user-specific information like location
(e.g. what are some good restaurants near me, vs in NYC, etc.) inferred from e.g. geolocation data 
or an understanding of user information that the provider has collected.

I used Grok 4.6 for most of this. It is still very much a work in progress. I also imagine GDM has implemented similar (likely better, 
more performant) versions of something along these lines. But I think there are likely many innovations to be made in this domain. 
I would be happy to collaborate with anyone who is interested! Feel free to send me an email, raise an issue, make a PR, etc.

You can find my contact info on my main GitHub page.
