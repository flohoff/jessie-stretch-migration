
The arch indep libgpg error .a files miss a correct ranlib run so
the build of libksba fails.

When building libksba arch-indep manually - run ranlib beforehand:

	x86_64-w64-mingw32-ranlib /usr/x86_64-w64-mingw32/lib/libgpg-error.dll.a
	i686-w64-mingw32-ranlib /usr/i686-w64-mingw32/lib/libgpg-error.dll.a

