---
layout: post
title: Creating a Dash docset for the NIST algorithm database
comments: true
category: articles
---

scraping wikipedia algorithms to make a docset

	curl 'http://en.wikipedia.org/wiki/List_of_algorithms' > /tmp/wiki.html
	cat /tmp/wiki.html | pup '#bodyContent #mw-content-text a[href^=/wiki/] attr{href}'
	
css changes

	#mw-page-base, #mw-head-base, #mw-navigation, #footer {
	display:none;
	}
	
	.mw-body {
	margin-left: 0px;
	}
	
	cat /tmp/wiki.html | pup '#bodyContent #mw-content-text a[href^=/wiki/] attr{href}' | tail -n+4 | (while read i; do wget -e robots=off -R skins/ -k -p -c -E -H -K --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" "en.wikipedia.org$i"; done)	
	
	npm install -g replace
	
	cd wiki/en.wikipedia.org/wiki
	replace -r en.wikipedia.org/wiki/ "" *
	find . -name \*orig -print0 | xargs -0 rm
	
checkig if already existing

	cat /tmp/wiki.html | pup '#bodyContent #mw-content-text a[href^=/wiki/] attr{href}' | tail -n+4 | (while read i; do if [[ -f en.wikipedia.org$i.html ]]; then continue; fi; echo "no $i"; wget -4 -e robots=off -R skins/ -k -p -c -E -H -K --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" "en.wikipedia.org$i"; done)
	
	alias urldecode='python -c "import sys, urllib as ul; print ul.unquote_plus(sys.argv[1])"'
	
	for i in *; do mv "$i" "$(urldecode $i)"; done
	
	for i in en.wikipedia.org/wiki/*html; do ../index.sh "$i"; done