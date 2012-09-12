![Creative Principle](http://creativeprinciple.com.au/logo2.png)
#Apache output compression

	# compress text, html, javascript, css, xml:
	AddOutputFilterByType DEFLATE text/plain
	AddOutputFilterByType DEFLATE text/html
	AddOutputFilterByType DEFLATE text/xml
	AddOutputFilterByType DEFLATE text/css
	AddOutputFilterByType DEFLATE application/xml
	AddOutputFilterByType DEFLATE application/xhtml+xml
	AddOutputFilterByType DEFLATE application/rss+xml
	AddOutputFilterByType DEFLATE application/javascript
	AddOutputFilterByType DEFLATE application/x-javascript

	# Or, compress certain file types by extension:
	<files *.html>
	SetOutputFilter DEFLATE
	</files>
	
## mod_gzip configuration for Apache 1.3.x

	<IfModule mod_gzip.c>
	mod_gzip_on Yes
	mod_gzip_dechunk Yes
	mod_gzip_item_include file \.(html?|txt|css|js|php)$
	mod_gzip_item_include handler ^cgi-script$
	mod_gzip_item_include mime ^text/.*
	mod_gzip_item_include mime ^application/x-javascript.*
	mod_gzip_item_exclude mime ^image/.*
	mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
	</IfModule>

## mod_deflate configuration for Apache 2.x

	<IfModule mod_deflate.c>
	SetOutputFilter DEFLATE
	# Dont compress
	SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-gzip dont-vary
	SetEnvIfNoCase Request_URI \.(?:exe|t?gz|zip|bz2|sit|rar)$ no-gzip dont-vary
	#Dealing with proxy servers
	<IfModule mod_headers.c>
	Header append Vary User-Agent
	</IfModule>
	</IfModule>

## Verify Your Compression

Online compression checks.

 * http://www.gidnetwork.com/tools/gzip-test.php
 * http://www.port80software.com/tools/compresscheck.asp
 
View the headers: https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/.

##References
* http://httpd.apache.org/docs/2.0/mod/mod_deflate.html