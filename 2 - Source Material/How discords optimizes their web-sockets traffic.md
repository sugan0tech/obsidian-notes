#backend 
#web 

	since 2017, discords server WS response is compressed using zlib, making messages anywhere from 2 to 10 times smaller. So the case is like them to move from zlib to much better one zstandard ( recent compression algo )

### Zstandard
- Unlike other compression algos like zlib, i uses dictionary for compression
- So with larger data, better the compression ratio be
- `Learns from past data to compress future data`
	- There is a training mode for optimize zstandard

> Here for discords case, all of their compression for small bytes of data, which zlib aces at

> Training works if there is some correlation in a family of small data samples. The more data-specific a dictionary is, the more efficient it is (there is no _universal dictionary_)

# Problemos
- But their gateway was in elixr ( on erlang ), there was no binding streaming with `zstandard`  on [eztd](https://github.com/silviucpp/ezstd), so they made the required changes with a [pr](https://github.com/silviucpp/ezstd/pull/15) to upstream 


## Benefits
- with initial i.e tryout with zstandard was not better than zlib, but after the integration of streaming support with eztd. They got the expected performance boost `this all with default configuration`
- With config optimization
## References
 [How Discord Reduced WebSocket Traffic by 40%](https://discord.com/blog/how-discord-reduced-websocket-traffic-by-40-percent)
[Zstandardzst awesome compression library for the web](https://facebook.github.io/zstd)