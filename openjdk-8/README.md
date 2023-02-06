
Transition issues
=================

openjdk-8 in stretch only builds with openjdk-8 so there was a transition 
via a temp openjdk-8 package which had build alternates to openjdk-7

So build a backport jdk8 with jdk7

Another issue is that openjdk-8 checks for the kernel version and that
check prohibits building on kernel newer than 4.x.

For the transition from *snapshot.debian.org* we use a uname kernel version faker.


Stage 1 
========

	sbuild \
		--setup-hook="mv /bin/uname /bin/uname.real ; wget https://github.com/flohoff/jessis-stretch-transition/openjdk-8/uname.fake.kernel2.6 -O /bin/uname ; chmod 0755 /bin/uname" \
		--resolve-alternatives \
		--extra-repository='deb-src [trusted=yes] http://snapshot.debian.org/archive/debian/20170424T033346Z sid main' \
		openjdk-8_8u121-b13-4.1

Problem with kernel version
===========================

	>&2 echo "*** This OS is not supported:" `uname -a`; exit 1;
	*** This OS is not supported: Linux loong 5.10.0-21-loongson-3 #1 SMP PREEMPT Debian 5.10.162-1 (2023-01-21) mips GNU/Linux
	make[6]: *** [check_os_version] Error 1
	/<<PKGBUILDDIR>>/src/hotspot/make/linux/Makefile:238: recipe for target 'check_os_version' failed
	make[6]: Leaving directory '/<<PKGBUILDDIR>>/build/hotspot'
	/<<PKGBUILDDIR>>/src/hotspot/make/linux/Makefile:275: recipe for target 'linux_mipsel_zero/debug' failed
	make[5]: *** [linux_mipsel_zero/debug] Error 2
	make[5]: Leaving directory '/<<PKGBUILDDIR>>/build/hotspot'
	make[4]: *** [generic_buildzero] Error 2

hotspot src checks for the kernel version which needs to be 2.2*), 3*), 4*) - everything else will be denied. Comment
says this is because of people should not build with < 2.2.

