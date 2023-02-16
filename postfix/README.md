
Postfix checks for kernel version - As i am running a 5.10 kernel with a stretch chroot
this breaks build.

Use our fake uname:

	sbuild \
		--setup-hook="apt-get update && apt-get -fy install wget ; mv /bin/uname /bin/uname.real ; wget --no-check-certificate https://raw.githubusercontent.com/flohoff/jessie-stretch-migration/main/openjdk-8/uname.fake.kernel2.6 -O /bin/uname ; chmod 0755 /bin/uname" \
		postfix_3.1.15-0+deb9u1
