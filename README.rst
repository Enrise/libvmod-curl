=========
vmod_curl
=========

-------------------
Varnish Curl Module
-------------------

:Author: Martin Blix Grydeland
:Date: 2011-05-26
:Version: 1.0
:Manual section: 3

SYNOPSIS
========

import curl;

DESCRIPTION
===========

This vmod provides cURL bindings for Varnish so you can use Varnish
as an HTTP client and fetch headers and bodies from backends.


FUNCTIONS
=========

Please see man/vmod_curl.rst

INSTALLATION
============

The source tree is based on autotools to configure the building, and
does also have the necessary bits in place to do functional unit tests
using the varnishtest tool.

Usage::

 ./configure VARNISHSRC=DIR [VMODDIR=DIR]

`VARNISHSRC` is the directory of the Varnish source tree for which to
compile your vmod. Both the `VARNISHSRC` and `VARNISHSRC/include`
will be added to the include search paths for your module.

Optionally you can also set the vmod install directory by adding
`VMODDIR=DIR` (defaults to the pkg-config discovered directory from your
Varnish installation).

Make targets:

* make - builds the vmod
* make install - installs your vmod in `VMODDIR`
* make check - runs the unit tests in ``src/tests/*.vtc``

In your VCL you could then use this vmod along the following lines::
        
	import curl;

	sub vcl_recv {
	    curl.fetch("http://example.com/test");
	    if (curl.header("X-Foo") == "bar") {
	        …
	    }
	    curl.free();
	}

HISTORY
=======

Development of this VMOD has been sponsored by the Norwegian company
Aspiro Music AS for usage on their WiMP music streaming service.

PACKAGING
=========

DEBIAN / UBUNTU
---------------

To build the Debian package::
	./mk-debian-binarypackage.sh

The use of this shell script is not ideal as it does not allow Canonical to build a package
(which it attempts to do based on source packages). If you fix this, we will gladly accept
your Pull Request =)

REDHAT / CENTOS
---------------

To package this vmod you need to build Varnish first. Please refer to
the Varnish build instructions for a complete how-to. After that you
can build the packages.

To build Varnish under CentOS::

	yum install rpm-build redhat-rpm-config make gcc git yum-utils libtool pcre-devel python-docutils curl-devel
	git clone -o upstream git://github.com/varnish/Varnish-Cache.git
	cd Varnish-Cache
	git checkout varnish-3.0.2
	./autogen.sh
	./configure
	make

Then, to build the package::

	redhat/make-tarball
	rpmbuild -bb --define 'VARNISHSRC /root/Varnish-Cache' redhat/*spec

If all went well, your RPM's will have ended up somewhere in `/root/rpmbuild/RPMS/x86_64`


COPYRIGHT
=========

This document is licensed under the same license as the
libvmod-example project. See LICENSE for details.

* Copyright (c) 2011 Varnish Software
