SDCCPATH=/opt/sdcc/
SDCCBIN=$(SDCCPATH)/bin/
SDCCEXTRA=$(SDCCPATH)/share/sdcc/

SDCC=$(SDCCBIN)/sdcc
SDCCINC=$(SDCCEXTRA)/include/mcs51/
SDCCLIB=$(SDCCEXTRA)/lib/large/
SDCCMFLAGS=-mds390 --use-asix --opt-code-size --stack-size 0x80
SDCCLFLAGS=--xram-loc 0x0000 --xram-size 0x8000 --iram-size 0x80 --code-loc 0x000000 --code-size 0x080000 --stack-loc 0x80 --idata-loc 0x30

OBJECTS=cpu_ax110xx.rel interrupt_ax110xx.rel main.rel uart/uart.rel

all : asix.bin

asix.bin: asix.ihx
	$(SDCCBIN)/makebin -p <asix.ihx >asix.bin

asix.ihx: $(OBJECTS)
	$(SDCC) -I$(SDCCINC) $(SDCCMFLAGS) $(SDCCLFLAGS) $(OBJECTS) -o asix.ihx

cpu_ax110xx.rel: cpu_ax110xx.c cpu_ax110xx.h ax110xx.h
	$(SDCC) -I$(SDCCINC) $(SDCCMFLAGS) -c cpu_ax110xx.c

interrupt_ax110xx.rel: interrupt_ax110xx.c interrupt_ax110xx.h ax110xx.h
	$(SDCC) -I$(SDCCINC) $(SDCCMFLAGS) -c interrupt_ax110xx.c 

main.rel: main.c ax110xx.h interrupt_ax110xx.h cpu_ax110xx.h
	$(SDCC) -I$(SDCCINC) $(SDCCMFLAGS) -c main.c 

uart/uart.rel: uart/uart.c ax110xx.h
	$(SDCC) -I$(SDCCINC) $(SDCCMFLAGS) -c -o uart/ uart/uart.c 

clean:
	rm -f mon.ihx *.asm *.lst *.rel *.rst *.sym *.cdb *.map *.ihx *.lnk *.hex *.mem *.bin *~
	cd uart; rm -f *.asm *.lst *.rel *.rst *.sym *.cdb *.map *.ihx *.lnk *.hex *.mem *.bin *~; cd ..
