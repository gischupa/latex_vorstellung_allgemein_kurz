# Makefile für die Umwandlung von tikz-Dateien in PNG-Bilder

# Definiere den Ordner, in dem die tikz-Dateien liegen
TIKZ_FOLDER = .

# Definiere den Ordner, in dem die PNG-Bilder gespeichert werden sollen
PNG_FOLDER = png_images

# Liste alle tikz-Dateien im TIKZ_FOLDER auf
TIKZ_FILES := $(wildcard $(TIKZ_FOLDER)/*.tikz)

# Ersetze die Dateiendung .tikz durch .png für die PNG-Dateien
PNG_FILES := $(patsubst $(TIKZ_FOLDER)/%.tikz,$(PNG_FOLDER)/%.png,$(TIKZ_FILES))

# Regel für die Umwandlung von tikz in PNG
$(PNG_FOLDER)/%.png: $(TIKZ_FOLDER)/%.tikz
	@echo "Umwandlung von $< zu $@"
	mkdir -p $(PNG_FOLDER)
	echo "\documentclass{standalone}" > temp.tex
	echo "\usepackage{tikz}\n " >> temp.tex
	echo "\\\begin{document} " >> temp.tex
	cat $< >> temp.tex
	echo "\\\end{document}" >> temp.tex
	/usr/bin/pdflatex -halt-on-error -output-directory=$(PNG_FOLDER) temp.tex >/dev/null
	mv $(PNG_FOLDER)/temp.pdf $(basename $@).pdf
	convert -density 300 $(basename $@).pdf $@
	rm -f temp.tex 
	rm -rf $(PNG_FOLDER)/*.pdf $(PNG_FOLDER)/*.aux $(PNG_FOLDER)/*.log
	cd ..

# Standardziel: Alle PNG-Bilder erstellen
all: $(PNG_FILES)

# Regel für das Löschen aller temporären Dateien
clean:
	@echo "Lösche temporäre Dateien..."
	@rm -rf $(PNG_FOLDER)/*.png $(PNG_FOLDER)/*.pdf $(PNG_FOLDER)/*.log $(PNG_FOLDER)/*.aux temp.tex

# Phony-Ziel, um sicherzustellen, dass clean immer ausgeführt wird, auch wenn eine Datei namens "clean" existiert
.PHONY: clean
