---
layout: post
title: "Big Strings Come in Small Packages"
date: 2011-09-09
summary:  Just how much data can we compress into a small file of an everyday format? Turns out it's quite a bit.
---

<p>
Just how much data can we compress into a small file of an everyday format? Turns out it's quite a bit.
</p>

<p>
A couple of years ago I <a href="/blog/crafting-an-effective-hotlink-response-part-1">tried to battle hotlinkers by weaponizing image files</a>.
By substituting the original image with a <a href="http://en.wikipedia.org/wiki/Shock_site">shock site</a>-type image
or a large annoying high-contrast pattern I hoped to disrupt the offending sites' layout, readability and/or sense;
hopefully resulting in complaints and the link getting pulled.
That didn't work, but while reading about <a href="http://en.wikipedia.org/wiki/Dazzle_camouflage">dazzle camoflague</a>
and constructing intentionally annoying image files I noticed something.<!--div style="float:right; margin:0.5em; margin-right:0; border-color:#eee">
<img src="/i/mg/deceptively-small/checkerboard-256.png" height="256" width="256" alt="256x256 diagonal" style="border:1px solid #999; margin:0; padding:0">
<div style="background-color:inherit">256&times;256 PNG: 162 bytes</div>
</div-->
Highly-repetitive-pattern images compressed extremely well in the
<a href="http://en.wikipedia.org/wiki/Portable_Network_Graphics">PNG</a> format;
so well that a screen-filling 1024&times;1280 PNG compressed into a 1K file.
In many cases these screen-sized images were smaller in size than the HTTP headers used to send them.
<p>
Taking this to its logical conclusion: how large of an image can we fit into a a suitably small PNG file?
Using a 1-bit color depth and a single color and <a href="http://www.gimp.org/">GIMP</a>:
<table class="border" cellspacing="1">
<tr>
  <th>Dimensions
  <td>1<sup>2</sup>
  <td>256<sup>2</sup>
  <td>2048<sup>2</sup>
  <td>4096<sup>2</sup>
  <td>8192<sup>2</sup>
  <td><a href="/i/mg/deceptively-small/white-16384x16384.png">16384<sup>2</sup></a>
<tr>
  <th>Pixels
  <td class="r">1
  <td class="r">64K
  <td class="r">4M
  <td class="r">16M
  <td class="r">64M
  <td class="r">268M
<tr>
  <th>Bytes
  <td class="r">95
  <td class="r">116
  <td class="r">618
  <td class="r">2144
  <td class="r">8257
  <td class="r">32761
</table>
<p>
This means an image file could be transferred over a network in mere milliseconds
yet require seconds-worth of CPU usage to decompress, consume hundreds of MBs of RAM
and yet be essentially invisible to a user.
<p>
Browsers on Windows XP all react distinctly when displaying the <a href="/i/mg/deceptively-small/white-16384x16384.png">32K 16384&times;16384 PNG</a>:

<table class="border">
<tr>
  <td style="border:0">
  <th>CPU
  <th>RAM
  <th>Interpretation
<tr>
  <th>Chrome 13
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAAMElEQVRIiWNgGAWjYBSMglEwCkbBKBgFBADjmTNn/sM4JiYmjDwMDHD+Fyzy9OQDAIz5JWVXyp9iAAAAAElFTkSuQmCC">
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAALElEQVRIie3HoQEAIAzEwJSZuv8EPxOgwCIxOXcgSZKkh0qyTrq7BtxP+PoNPukVAxWISJQAAAAASUVORK5CYII=">
  <td class="sm">barely a blip
<tr>
  <th>IE6
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACsAAAAQCAYAAACP4IauAAAABmJLR0QA/wD/AP+gvaeTAAAAXUlEQVRIiWNkIBH8Z2D4D2MzMjAwkqqfEsBET8soBaOOpRUYdSytwKhjaQVGHUsrMOpYWoFRx9IKDCnHspw5cwbe5DMxMWHkQWoCfmFgYGRF4v/G0iREl0c3j5p8ACTxI4U4kobsAAAAAElFTkSuQmCC">
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAAW0lEQVRIie2OQQqAMAwEp6Uv0vz/BfFL6qmCFRHXIiKZUxayyUDwQ9LdgsNUZ4Oxr86eInSG7hYn5LceKYScSsiphJxKyKl8Wq64+1KDmaUMW57hkNsDV/tP8grbwxVRT+xMwgAAAABJRU5ErkJggg==">
  <td class="sm">spikes briefly but returns to baseline quickly
<tr>
  <th>Opera 11.51
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACsAAAAQCAYAAACP4IauAAAABmJLR0QA/wD/AP+gvaeTAAAAqklEQVRIie2UzQ0CIRBG3xiPVrNShm1oLGPLMNaxZYxW4x1PmAmHZXHElcR3mkn4eQT4hEoixFQLyDfnb2o3W5OuZLelAQq3VAfYt9WZpygLDJ4NJnPAgzn4OyyRdRFBTVv9IS1u2QtcU32Gk3e9OdyyAkfT/rZszmhydHRee05X0fWXbUXz6CoxmBy+F3J4dVmpyOG+noGqvqImhCA7Ez2PD0fPEnIf2z8BjWsfLUOB1xkAAAAASUVORK5CYII=">
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAAYUlEQVRIiWNgGAXDEDCSquEMA8NZGNuEgcEYnU8thzEwMDCwkKHHiACfaoCJVgZTA4w6jlyAkeYaGBj+I7EZ0fmEDCSknxT+oA65UceRC0YdRy4Y1I5jYULKuv/IqGtpCQDrnxQDPMkA5AAAAABJRU5ErkJggg==">
  <td class="sm">several seconds heavy CPU, permanently allocates ~256MB
<tr>
  <th>Firefox 6
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAAn0lEQVRIiWNkQAP/GRj+w9iMDAyM6PL0BEwDaTkhMOo4cgELtQ08gJRmHRgYGNH5hNTT1HGEwFIGhjQYO5qBYRY+tXR3HCMDw0wkLl7HDa8014CURhpoXA4O6pAbdRy5gOLcasTAYAxjn2NgOEupeciAYscxMjCcQeVSDxB0nCRSWfQcqQClB2A5c+YMvGgwMTHB5vNUJDbNHceEVFQBAFDnF/7qxUpYAAAAAElFTkSuQmCC">
  <td><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAYAAACV3GYgAAAABmJLR0QA/wD/AP+gvaeTAAAATUlEQVRIiWNgGMSAEV3gGQNDGowtxcAwayD52Bz3H0kx40DymdAdN5jAqOPIBaOOIxeMOo5cMOo4csGo48gFg9pxLExIFe0/LA2BgQQAGQAlUAnpUJMAAAAASUVORK5CYII=">
  <td class="sm">heavy, sporadic CPU, allocates ~1GB RAM
</table>
<p>
After some research I realized that I had just rediscovered the decompression bomb.
<p>
<div style="float:left; padding:0.5em">
<img alt="&#x1F4A3;" title="zip-bomb" width="64" height="64" src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAMCAgICAgMCAgIDAwMDBAYEBAQEBAgGBgUGCQgKCgkICQkKDA8MCgsOCwkJDRENDg8QEBEQCgwSExIQEw8QEBD/2wBDAQMDAwQDBAgEBAgQCwkLEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBD/wAARCABAAEADASIAAhEBAxEB/8QAHAAAAgMBAQEBAAAAAAAAAAAABQgGBwkEAAEK/8QAPBAAAQQBAgUCAwQFDQEAAAAAAQIDBAUGBxEACBITISIxFEFRCSMyYRUWGHGBJDM0Q1JWYnKRlJWx1NP/xAAaAQEAAgMBAAAAAAAAAAAAAAAFAwQBBgcC/8QALhEAAQMEAQIDBgcAAAAAAAAAAQIDEQAEBSExElFBYZEGFCJxgdETFUKCseHw/9oADAMBAAIRAxEAPwB8NSuaLTTCK2rl0F9WZbJsJ7LK4lJObmPMwgeqTKKGipRS20lRA29SyhI8q4lMfXXRmZVO3UHVHGJUZmO5KWI9m066G0JKl/dJJc6gAd0dPVuNtt/HCSv5tilRqpm68GyKmjRxklor4VqOxJeKUlsOL6CpC1MF1MtWzTgVu42oBaOoGDZ5zE4hy5SWrh3Tqsy7IM9x1iQaq5eZktrrX0uLZkTEBv8AkaytLThithSHUOdPoLYd41PDZm/yeRfaWhsMNmNLJXIKgSU9IgGAQZiCIJMgbdmsTZYu2QnodDhAIUQAlRUkGP2yQSJJIMgapoMu5ndYYl9JapcdxulgEhUaHdQpD85LZ/Cp/tyG0trV7lsAlG/SVEg8Bv2ptdPpgP8AxEz/ANnEfwzmR+zye0exXP8AUbF9KKW9t4KTY0ULFWZkuLNQS2+n4Zhlx5truoX21uABSOk9R9+OvF+Z/wCzByixbqxQ4FUPPLCEOXGBiHH/AHqfXG7TY/Na0jjqTOTwyG0pcsuogbP4hE+cVzF3H5ZaypF3APh0DXlzRUc02um/4cCP7qiZ/wCzi2tPuafA76kQM7ms4xkEdXalwnUuLZdUB4ejuBPraV7jf1JO6VDcbkxI5euXbL8c66TTXDY8azjpdh29DWxY76EqAU2/HksoBB9lBSTsfnuCQaMhY9nuhGdQae8xC1yaLZy1RKm2q1w225pS0t4JcS8+2GXg204Sk+glBKSd+kTlWEybKktNlhxOx8RUFeW+DVc/m+NcC3HA82edBPT568KYB/mL0Tjf0nUSsa/zhxP/AGniYYrl2M5vTt5BiV3Fta91a20yIy+pPWg7KSfmCCPY8LJq9O1CzS1raus0aydqXad1URp+wqAXA0kKc8pmkDYEe5G/y4trlnw3KsI02XW5jTLqbGVbTZphuPtPLabcc9HUpla29yBv6VHwR89wC7uwtGLND7bsuE7T2HekbO+ubi5U0tqGwJCu9FNTdC8A1KwCdgkqhgQEOMKTAlRYyG3K9/Y9t5opA2KTsdvYjcHwTxnf9q3pbo9g7OCW2Px1V2oly0IcwwwkMy6yIwhrvvpP9ak9httXuU9QPUGwBqpxjl9q9OsJPNcxEmFQYh4fWIiJJ8dCpExSlAfmsqBP+EfTgMNpCusDfHpTqn3VNBlSj0gkgeEmJP1gelJqyw1HR22k9I33PzJP1J9yfzPF2ctuhFNrRMunchu5kKDTpYSWoJQl91x3r2PUtKglI7Z39J33HttxS/BjGMwynCp67PE76bVSXGy045FdKOtHv0qHsob7HY/MA8e6hpxuV/Xy75NuYlehN9lDlpppcWLER1D6tk1T0pKFtS2xvs3sp1KX0jZKgVObdSQC+vM5Pv4eQYSO/aRqhMuRIiP1MaKuSLRMZ5AQpctQYQlUZ2SodY23bVv7DjC67nzbVc2ytpr8qVK7jsiQ+4VuOKIJKlKPkn8zxs3qeizyRvSa2FELvKJWMNOWcORStTvh2XGW1mSyX3Wmm3g+kNndXlLu5/AkKt2Il9NUckYtV1G8lzTHLFppic9ZXlgyxJAlTc7Up2s9KepbkahZfSkn0nYp29O58DcOFhzdk1iNG1dWjNnYIrYyZc1jftyXg0nrdRv56VK3UN/Ox4VXKbvO6qoQxdakQKGuYYe71ReZVV1Ljnp9LbLcCOpYO+42U+rfceRsepntNjTnTrFjjte9AqjSwfgYjylKcjx+wjttqK/UVJTsCVedx588SXvA+ZqOw4+gqScZ7faxcu11luP0vMFiNa7NkYnFcrsgYYQVOGsUsuNyAB5IZcLnVsPwOqV7J40J4HZDcUlBSTLfJJkeLVxmiqU9I/m0t+x6vy87fx4oAToUlxX5vELS4kLQoKSobgg7gjj7xpfrjydcj2fXkvIMC1jb05tJS1OSIcJvvVy3CdyoRlpHb3+jakp/LitcV5B+W1mxQ7nPOG3YQEqBVHqqtMNxwf2S4subD9wB+hHEwtnjsIPoah94aH6h6il25VuX685lNY6nA4MJ1dHFeam5LMCT24telW60FXsHHQC2hPuSoq22QrbWPmnqW5+TYLWz6llFW2JSYb6KB61XKk9vZUAtoWEIQWh3wHAoKMbfwG1BX3TbOOUPly0+dxrSGZXiPHBe+BrULkT7OSdgCpa/U64o7DrWoBI8kpSCRCKa8zHW3MY9jnOV2VWiLM+MrqypfbbYrSWltJCVqbKnV9t1wKcV+IrVslKelKVsZibt4quAiEoEknX0Hc0TlclbNoFv1/GvQjfr5f4V2s4RKS/VUtHi2eQLSczLFQtiRTY5FbKW0lxTf6OAkJAHT4d999vIJHDU4u3fs4zUNZW7Gcu0QI6bJcUnsqlBtPdLe+x6Cvq2387bcK5rFSXGIWUCwp9TctMmu7giuuzmVKaDgAXts0PcADz9OLZ5XcnyPK9MnZuT3km2lxridERKk9JcU0hz0hRSBvtufP08fLjOQxr7dmi+VHQowO/esY2/ZXcKsxPWkSe1W9xyWtVWXtZKprqvjzoE5lceTGkNhxp5tQ2UhST4IIJBB46+PcBU7SxZZyqZ5LvH38Z1BrXas7CKm6jvvy2WwPDa3kr++6fYLUOsp26ypQK1B/2StWf78Yl/sJP/ANOG149w+17T5ZhAbQ8QAIGh9qEc9ncY8suLaEnZ2fvWdFJR5bKs40fKsjxnGa63hOWVNYzWluNzYyHEo3Uht5S2VnrQpKFjyAvyOnySzem0mixK7DcYo4OcXVfJjW9xlzwjBx5TiXm1RWWn2XkJZCB6WjulJCFHrcBWbz045cdSa2ZH/W7NxSxqyqFZDcxyY2888nu9X3qZUNSEDYDwgk7/AD24GZ3yyaqS8zlXeMZPWX8OTCjRw9kdiY0ttTanSU7RYRbKPvdwdgffffxwwjLt31yhvJXJLY3oQJjXAk71xQj+LuLS1WvG2yQ4dCTJid8mBrfPbxqq6bT7HLkQrpnSeFMqKiQmZYQ5DdG0ZZaIW02VtVyV9suJSHE9XqQVJO4JBv3TrmH0qj4zBg4Xp7MqqqJ8EmyYp4cVmDSvzZBaShaetsr3d6iVNNq8HcgHcAJjmjuvGP0k6mFLgT4mjbufrLMT0fw/R53/ANeAdLyuay0yWKteV45OrpaKb45Tkh5pcMRJan1sspSwQ+kFaulay2T1eQNt+KGRTi3VqFu5qTEzxHy5Jkf1VvFqyzLafeWtwJiNGYPidAb/AI3X/9k=">
</div>
A <a href="http://www.aerasec.de/security/advisories/decompression-bomb-vulnerability.html"><dfn>decompression bomb</dfn></a>
is an attack whereby a compressed file is carefully crafted to decompresses into
pathologically large output with the goal of overwhelming the receiver with data.
It can be an end in itself but may also be intended to divert archive-scanning
antivirus systems from the true attack.
They have been around since at least the 1980s. They come in several flavors:
<a href="http://en.wikipedia.org/wiki/Zip_bomb">Zip bombs</a>,
<a href="http://en.wikipedia.org/wiki/E-mail_bomb">Email bombs</a> and
<a href="http://en.wikipedia.org/wiki/Billion_laughs">XML bombs</a>.
<p>
Could today's widespread file formats be susceptible to a similar attack?
<p>
<a href="http://libpng.sourceforge.net/decompression_bombs.html">Libpng was susceptible to decompression bombs</a> until quite recently</a>
and any applications either using an older version or not following the new recommendations still will be.
What other widely used software may be susceptible to decompression bombs?
<p>
<h2>General Applicability</h2>
Several standard components of the WWW rely on compression:
<ul>
  <li>JPEG image format
  <li>PNG image format
  <li>GIF image format
  <li><a href="http://en.wikipedia.org/wiki/Gzip#Other_uses">HTTP/1.1 supports a <em>gzip</em> <code>Content-Encoding</code> header</a>
</ul>

When responding to an HTTP request (say for a hotlinked image), we might carry out an
asymmetric-style attack by returning gzip'ed HTTP content which would decompress into an
enormous file, potentially consuming disproportionate CPU and memory on the client's machine.
We'll throw in the less-widely-used but formidable <a href="http://en.wikipedia.org/wiki/Bzip2">bzip2</a> for comparison.
<p>
The following script generates streams of zero bytes (<code>'\0'</code>), compresses them and saves them to disk:
<pre><code>#!/bin/sh
zeroes() {
  dd if=/dev/zero bs=4096 count=$1;
}
blocks=1024
for n in $(seq 1 9)
do
  bytes=$((4096*blocks))
  zeroes $blocks | bzip2 - > zero-$bytes.bz2
  zeroes $blocks | gzip - > zero-$bytes.gz
  blocks=$((blocks*2))
done</code></pre>
<p>
<h4>Results</h4>
<table class="border">
<tr>
  <th>Original Size
  <th>4MB
  <th>8MB
  <th>16MB
  <th>32MB
  <th>64MB
  <th>128MB
  <th>256MB
  <th>512MB
  <th>1GB

<tr>
  <th>Gzip'ed
  <td>4KB
  <td>8KB
  <td><a href="/i/mg/deceptively-small/z/zero-16MB.gz">16KB</a>
  <td>32KB
  <td>65KB
  <td>130KB
  <td>260KB
  <td>512KB
  <td>1MB

<tr>
  <th>BZip2'ed
  <td>48
  <td>48
  <td><a href="/i/mg/deceptively-small/z/zero-16MB.bz2">45</a>
  <td>46
  <td>79
  <td>112
  <td>208
  <td>402
  <td>785

</table>
<p>
Bzip2 performs astoundingly well, compressing streams of zero bytes (minus overhead) by a factor of &cong; 1 million(!).
If HTTP supported bzip we could easily construct a custom response the size of an icon which when
decompressed would tie up a client's CPU and a large portion of their memory.
<p>
Gzip performs logarithmically, compressing the original by a factor of &cong;1000.
This performance is not close to bzip2's, but is enough to consider trying a decompression bomb.
Let's build a test HTTP response using a gzip'ed stream of zeroes and see how much RAM a web
browser uses when using it.
<p>
<h3>See Also</h3>
<ul>
  <li><a href="http://www.aerasec.de/security/advisories/decompression-bomb-vulnerability.html">Decompression bomb vulnerability</a>, AERAsec
  <li><a href="http://solitude.vkps.co.uk/Archives/2006/01/08/decompressionbombs/">Decompression bombs</a>, Solitude
  <li><a href="http://secunia.com/advisories/38774">Secunia Advisory SA38774: libpng Ancillary Chunks "Decompression Bomb" Denial of Service</a>
  <li><a href="http://en.wikipedia.org/wiki/Kolmogorov_complexity">Kolmogorov complexity</a>
</ul>
