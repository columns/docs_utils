
obj-m:= drv_sendsignal.o
KDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)
SIGNAL := 14

all:
	@echo "--> Making userspace program"
	@gcc usr_getsignal.c -o usr_getsignal
	@file usr_getsignal
	@echo "--> Making kernelspace driver"
	@make -C $(KDIR) M=$(PWD) modules
	@file drv_sendsignal.ko

install:
	@make -C $(KDIR) M=$(PWD) modules_install
	@depmod -a

info:
	modinfo drv_sendsignal.ko

clean:
	@echo "Cleaning the environment "
	@make -C $(KDIR) M=$(PWD) clean
	@rm -f ./usr_getsignal
	@rm -f  /tmp/my_pid.txt

run:
	@clear
	@echo "--> Running userspace program in background"
	@./usr_getsignal $(SIGNAL) &
	@echo `ps aux | grep usr_getsignal | grep -v grep | cut -f2 -d" "` > /tmp/my_pid.txt
	@echo "PID used by usr_getsignal : `cat /tmp/my_pid.txt` "
	insmod ./drv_sendsignal "`cat /tmp/my_pid.txt`"
	@dmesg
	@echo "Done."

