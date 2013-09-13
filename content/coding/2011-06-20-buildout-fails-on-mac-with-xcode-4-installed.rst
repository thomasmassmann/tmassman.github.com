:title: Buildout fails on Mac with Xcode 4 installed
:summary: Installing Apple's Xcode 4 can cause some trouble when compiling
          Zope packages via buildout. This article shows the solution for
          this problem.
:tags: MacOS, Buildout, Python

**Installing Apple's Xcode 4 can cause some trouble when compiling Zope
packages via buildout. This article shows the solution for this problem.**

Xcode4 dropped PPC support. After upgrading to Xcode 4 I realized some strange
behaviors when running buildout (especially when creating a new Zope buildout
where things have to be compiled).

I got some errors like this::

    compilation terminated.
    lipo: can't open input file: /var/folders/8q/8q7FGaB3FHq8zCFUe4wSVk+++TI/-Tmp-//ccfKEKaS.out (No such file or directory)
    error: command 'gcc-4.2' failed with exit status 1

The solution to solve this is pretty simple. Just add this line to your .profile in your home directory::

    export ARCHFLAGS="-arch i386 -arch x86_64"

That's it.
