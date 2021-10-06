theme=mywhite
reveal=reveal.js-4.1.2
theme_css=${reveal}/dist/theme/${theme}.css
previous_theme=$(shell ack -o ${theme_css} index.html)

.PHONY: all
all: switch_theme index.html

.PHONY: switch_theme
switch_theme:
ifneq (${previous_theme}, ${theme_css})
	touch ${theme_css}
endif

index.html: slides.md Makefile ${theme_css}
	pandoc -t revealjs -s -o $@ $< -V revealjs-url=./${reveal} -V theme=${theme}
