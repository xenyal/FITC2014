FITC2014
========


###### About [FITC](http://fitc.ca/)
### Future. Innovation. Technology. Creativity.

FITC produces design and technology events worldwide which inspire, educate and challenge attendees. Since 2002, FITC has brought together like-minded professionals and students in Toronto, Amsterdam, Tokyo, San Francisco, Chicago, Seoul, New York, Los Angeles and many other cities. Staying fresh by covering relevant topics in interactive, technical, design and business related content, FITC Events provides the professional development and networking opportunities needed to keep attendees up to date on the rapidly changing industry they work in.


## Page Speed: Reduce Network Delays

#### Context
- Average worldwide roundtrip time to google is ~100ms
- Mobile RTT is 100-1000ms
- If cellular radio is idle, add 1000-2000ms

#### Concurrent Connection Limits
- Maximum connections to one server/proxy: Less than 10
- Faking multiple domains from one server is workaround for max connections but is bad practice 
- Causes performance issues esp for mobile

## Image Compression
Paradigm is moving from raster to vectors, that is - going from JPEG/PNG to SVG. In terms of software, moving from Photoshop to Illustrator.

#### JPEG
- JPEG over all other image types (GIF/PNG/Whatnot)
- JPEG colors have different data requirements: 
- Blue (400nm) - Red (700nm) -> We don't like red because it's less chunky data
- Greyscale compression is poor
- FileOptimizer for Windows, ImageOptim for MacOSX, PUNYpng on the web are lossless optimization software that reduces size between 15-25%

#### PNG
- FileOptimizer for Windows, Smush.It on the web. Other freeware are lossy which will result in color/quality distortions.

#### SVG
- Tiny file footprint but can be scaled to any size
- Most browser supports it and software exports it
- Compressor.io will compress it but is lossy

#### Web Fonts
- Pros: Small file sizes - Renders crisply at any size
- Cons: One color - Overkill?
- Scales to any size
- Check out [IcoMoon](https://icomoon.io) for free shit

#### Base64 Encoding
- Embedding graphics as part of your page is a tradeoff
- 20% bigger file size, but reduces server connections
- Makes code hella ugly
```HTML
<img alt="" src="Q873847983264W2FJUSWHGFHJSKJASDKJF">

#### JPEG + PNG + SVG = Magic
- Will save a fuckton of space

```xml
<svg>
	<def>
		<mask>
			<img>
		</mask>
	</def>
</svg>

#### Sprites