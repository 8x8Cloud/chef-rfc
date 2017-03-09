---
RFC: unassigned
Title: My Title
Author: Alan Smithee <asmithee@example.com>
Status: Draft
Type: <Standards Track, Informational, Process>
<Replaces: RFCxxx>
<Tracking:>
<  - https://github.com/chef/chef/issues/X>
---

# Title

Merging chef-sugar platform and `platform_family` helpers, along with higher level helpers.

## Motivation

    As a Cookbook Author,
    I want the Chef Sugar helpers added to Core,
    So that everyone can use them.

    As a Chef Developer,
    The platform_family abstraction is limiting,
    A set of helper functions provides better abstractions.

    As a Cookbook Author,
    Helper methods produce cleaner case statement,
    Which produces more readable and maintainable code.

    As a Chef Developer,
    Helper methods can provide cleaner abstractions,
    Which should produce less need to break APIs.

## Motivation

There's considerable argument over what a `platform_family` should be, as evidenced by the
discussion over the amazon `platform_family` issue.  At the root of the problem seems to be
that every distro must belong to precisely one `platform_family` while the classification of
different distros forms more of an inheritance hierarchy.  The argument seems to be over at
which level of the hierarchy the `platform_family` should apply.

Helper methods solve this problem by allowing a given distro to respond as true to different
helper methods.

The chef-sugar helper methods have also found widespread adoption in the community and its
time to pull them into core.

## Specification

We will pull in most of the Architecture, Platform and Platform Family helpers from chef-sugar
unmodified, along with the `systemd?`, `docker?` and `kitchen?` helpers.  Since they have been
in widespread use, it is important to not modify their meaning.

### Architecture Helpers

* `_64_bit?`
* `_32_bit?`
* `i386?`
* `intel?`
* `sparc?`
* `ppc64?`
* `ppc64le?`
* `powerpc?`
* `armhf`?
* `s390x`?

### Platform Helpers

We will not be pulling in the `debian_before_squeeze?` style of `platform` and `platform_version` helpers
at this time (simply limiting the scope, not a statement that we do or do not eventually want them).

The complete list of platform helpers is long, examples:

* `amazon_linux?`
* `centos?`
* `linux_mint?`
* `oracle_linux?`
* `redhat_enterprise_linux?`
* `scientific_linux?`
* `ubuntu?`
* `solaris2?`
* `aix?`
* `smartos?`
* `omnios?`
* `raspbian?`
* `nexus?`
* `ios_xr?`

Note that this list deviates from the platform definitions in core chef, to eliminate ambiguity we will also
add helpers which precisely match the field in core chef:

* `linuxmint?`
* `oracle?`
* `scientific?`
* `redhat?`

Additional helpers will also be added to fill out the complete list of platforms, which will match the fields
used by core chef (matching the code in ohai).

Where there is a collision between `platform` and `platform_family` being named the same thing, and the platform_family
contains multiple distros, the platform version will be renamed:

* `debian_platform?`
* `fedora_platform?`

This is driven mostly by the widespread adoption of the `debian?` helper to mean "debian platform family".

For distros like "freebsd" where the platform, platform_family and os all match and there are no other derived classes
we shall use `freebsd?` to refer to the platform.

### Platform Family Helpers

Similarly we include the existing chef-sugar helpers:

* `arch_linux?`
* `debian?`
* `fedora?`
* `freebsd?`
* `gentoo?`
* `linux?`
* `mac_os_x?`
* `openbsd?`
* `rhel?`
* `slackware?`
* `suse?`
* `windows?`
* `wrlinux?`

### Additional Helpers From Chef-Sugar

These three helper will also be included:

* `docker?`
* `systemd?`
* `kitchen?`

The `docker?` helper has already been merged as of Chef 12.11.18

### Higher Level Helpers

In addition to the chef-sugar helpers, higher level helpers will be added:

* `rpm_based?` -- this will include the `platform_family` of `rhel`, `fedora`, `amazon`, and `suse`
* `fedora_based?` -- this will include the `platform_family` of `rhel`, `fedora`, and `amazon`
* `solaris_based?` -- this will include the `platform` of `openindiana`, `opensolaris`, `nexentacore`, `omnios`, `solaris2`, and `smartos`
* `bsd_based?` -- this will include the `platform` of `freebsd`, `netbsd`, `openbsd`, and `dragonflybsd`

Generally these helpers are designed to fit actual needs, not theoretical needs.  As an example, there exist families of gentoo-based and
arch-based systems but ohai does not support any of those other than the parent distro, so we do not define those helpers.

## Copyright

This work is in the public domain. In jurisdictions that do not allow for this,
this work is available under CC0. To the extent possible under law, the person
who associated CC0 with this work has waived all copyright and related or
neighboring rights to this work.
