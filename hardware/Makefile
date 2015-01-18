PCB=pcb
FILEBASE=epd
BOMFILE=${FILEBASE}.bom.txt
XYFILE=${FILEBASE}.xy.txt

default: gerber

gerber:
	${PCB} -x gerber --gerberfile ${FILEBASE} ${FILEBASE}.pcb
	mv *.gbr *.cnc ./epd-gerber

# backend bom and bom2 need a file "attribs" in the local folder
bom:
	#gnetlist -g partslist3 -o epd.bom epd.sch
	#gnetlist -g partslist1 -o epd.bom epd.sch
	#gnetlist -g bom -o epd.bom epd.sch
	gnetlist -g bom2 -o epd.bom epd.sch

pcb:
	gsch2pcb --use-files --elements-dir ~/footprints epd.sch

pdf: epd.pdf

%.pdf: %.ps
	ps2pdf $< $@

%.ps: %.sch
	gschem -p -o $@ -s /usr/local/share/gEDA/scheme/print.scm $<

# use the following command to generate colored ps file
# gschem -p -o epd.ps -s /usr/share/gEDA/scheme/print.scm epd.sch

# ps to png workflow
# ps --(ps2epsi)--> epsi --(eps2png)--> png
#
png: epd.epsi
	./eps2png -width 1800 epd.epsi

drc:
	gnetlist -g drc2 -o - epd.sch

clean:
	rm -f *.gbr *.bom *.cnc *.pdf ${BOMFILE} ${XYFILE}

