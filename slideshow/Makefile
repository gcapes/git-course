.PHONY: slides
slides: index.html

index.html: slides.md
	pandoc -t revealjs -s -o $@ $^ -V theme=mywhite
