[![Actions Status](https://github.com/lizmat/P5ref/workflows/test/badge.svg)](https://github.com/lizmat/P5ref/actions)

NAME
====

Raku port of Perl's ref() built-in

SYNOPSIS
========

    use P5ref; # exports ref()

    my @a;
    say ref @a;  # ARRAY

    my %h;
    say ref %h;  # HASH

    sub &a { };
    say ref &a;  # CODE

    my $r = /foo/;
    say ref $r;  # Regexp

    my $v = v6.c;
    say ref $v;  # VSTRING

    my $i = 42;
    say ref $i;  # Int

DESCRIPTION
===========

This module tries to mimic the behaviour of Perl's `ref` built-in as closely as possible in the Raku Programming Language.

HEAD1
=====

ORIGINAL PERL 5 DOCUMENTATION

    ref EXPR
    ref     Returns a non-empty string if EXPR is a reference, the empty
            string otherwise. If EXPR is not specified, $_ will be used. The
            value returned depends on the type of thing the reference is a
            reference to.

            Builtin types include:

                SCALAR
                ARRAY
                HASH
                CODE
                REF
                GLOB
                LVALUE
                FORMAT
                IO
                VSTRING
                Regexp

            You can think of "ref" as a "typeof" operator.

                if (ref($r) eq "HASH") {
                    print "r is a reference to a hash.\n";
                }
                unless (ref($r)) {
                    print "r is not a reference at all.\n";
                }

            The return value "LVALUE" indicates a reference to an lvalue that
            is not a variable. You get this from taking the reference of
            function calls like "pos()" or "substr()". "VSTRING" is returned
            if the reference points to a version string.

            The result "Regexp" indicates that the argument is a regular
            expression resulting from "qr//".

            If the referenced object has been blessed into a package, then
            that package name is returned instead. But don't use that, as it's
            now considered "bad practice". For one reason, an object could be
            using a class called "Regexp" or "IO", or even "HASH". Also, "ref"
            doesn't take into account subclasses, like "isa" does.

            Instead, use "blessed" (in the Scalar::Util module) for boolean
            checks, "isa" for specific class checks and "reftype" (also from
            Scalar::Util) for type checks. (See perlobj for details and a
            "blessed/isa" example.)

            See also perlref.

PORTING CAVEATS
===============

Types not supported
-------------------

The following strings are currently never returned by `ref` because they have no sensible equivalent in Raku: `REF`, `GLOB`, `LVALUE`, `FORMAT`, `IO`.

Also, since everything in Raku is a (blessed) object, you can only get the `SCALAR` response if you managed to put a Scalar container into another Scalar container (which is pretty hard), or you somehow have gotten ahold of a Proxy object. On all other cases, a Scalar container will be ignored and instead the contents of the container will be used.

$_ no longer accessible from caller's scope
-------------------------------------------

In future language versions of Raku, it will become impossible to access the `$_` variable of the caller's scope, because it will not have been marked as a dynamic variable. So please consider changing:

    ref;

to either:

    ref($_);

or, using the subroutine as a method syntax, with the prefix `.` shortcut to use that scope's `$_` as the invocant:

    .&ref;

AUTHOR
======

Elizabeth Mattijsen <liz@raku.rocks>

If you like this module, or what I’m doing more generally, committing to a [small sponsorship](https://github.com/sponsors/lizmat/) would mean a great deal to me!

Source can be located at: https://github.com/lizmat/P5ref . Comments and Pull Requests are welcome.

COPYRIGHT AND LICENSE
=====================

Copyright 2018, 2019, 2020, 2021, 2023 Elizabeth Mattijsen

Re-imagined from Perl as part of the CPAN Butterfly Plan.

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

