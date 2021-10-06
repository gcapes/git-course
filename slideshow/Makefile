theme=mywhite
reveal=reveal.js-4.1.2
theme_css=${reveal}/dist/theme/${theme}.css

previous_theme=$(shell ack -o ${theme_css} index.html)
ifeq (${previous_theme}, ${theme_css})
  different_theme=False
else
  different_theme=True
endif

.PHONY: all
all: touch_theme index.html

.PHONY: touch_theme
touch_theme:
ifeq (${different_theme},True)
	touch ${theme_css}
endif

index.html: slides.md Makefile ${theme_css}
	pandoc -t revealjs -s -o $@ $< -V revealjs-url=./${reveal} -V theme=${theme}
