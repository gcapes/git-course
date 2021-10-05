.PHONY: slides
slides: index.html

index.html: slides.md
	pandoc -t revealjs -s -o $@ $^ -V revealjs-url=./reveal.js-4.1.2 -V theme=sky
