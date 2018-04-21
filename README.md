# check_freenas_zpool_health

[check_freenas_zpool_health 0.01](https://github.com/freenas-monitoring-plugins/check_freenas_zpool_health)

This plugin uses FREENAS-MIB to query zpool health with SNMPv2c or SNMPv3

Tested with FreeNAS-11.1-U2, icinga 2.8.2, icingaweb 2.5.1, perl 5.26.1, Monitoring::Plugin 0.39, FreeBSD 11.1, Gentoo 2.4.1

## Usage

        Usage: check_freenas_zpool_health -H <host> -C <community> -z <zpool> -w <warning> -c <critical> -t <timeout> [-U <secname> -A <authpassword> -X <privpasswd> -a <authproto> -x <privproto>]
        
         -?, --usage
           Print usage information
         -h, --help
           Print detailed help screen
         -V, --version
           Print version information
         --extra-opts=[section][@file]
           Read options from an ini file. See https://www.monitoring-plugins.org/doc/extra-opts.html
           for usage and examples.
         -c, --critical=INTEGER
           Exit with CRITICAL status if type equal INTEGER [0..5]
         -C, --community=STRING
           SNMP community
         -H, --hostname=STRING
           Hostname to query - required
         -w, --warning=INTEGER
           Exit with WARNING status if equal INTEGER [0..5]
         -z, --zpool=STRING
           zpool name
         -U, --secname=STRING
           SNMPv3 username
         -A, --authpassword=STRING
           SNMPv3 authentication password
         -X, --privpasswd=STRING
           SNMPv3 privacy password (passphrase)
         -a, --authproto=STRING
           SNMPv3 authentication proto [MD5|SHA]
         -x, --privproto=STRING
           SNMPv3 privacy protocol [AES|DES]
         -t, --timeout=INTEGER
           Seconds before plugin times out (default: 15)
         -v, --verbose
           Show details for command-line debugging (can repeat up to 3 times)

## Notes:
  To use 'AES' SNMPv3 privacy protocol Crypt/Rijndae perlmod installation is
  required on the running host.
  Use '-vvv' debugging to troubleshoot any SNMPv3 encountered communication
  problems.

  According to FREENAS_MIB, managed zpool health types are:
    online(0),
    degraded(1),
    faulted(2),
    offline(3),
    unavail(4),
    removed(5)

  Thresholds are managed according to these values.
  Then use of '-w 0' will throw WARNING only if health value is greater than 0.
  It means WARNING will be raised if zpool status is 'degraded' or worst 
  (faulted, offline, unavail, removed).

## Examples:
  CHECK ZPOOL HEALTH WITH SNMPv2: check_freenas_zpool_health -H myfreenas.example.com -z raid -w 0 -c 1 -C public

  Check the zpool named 'raid' from myfreenas.example.com by using 'public'
  community and raise warnings if 'raid' zpool status is greater than "ONLINE"
  (0) and critical if greater than "DEGRADED" (1).

  CHECK ZPOOL HEALTH WITH SNMPv3: check_freenas_zpool_health -H myfreenas.example.com -z raid -w 0 -c 1 -U myuser -A mqlkdqfmLIHMOyçè67sdf -X yYOOJMohimoç96e283 -a SHA -x AES

  Check the zpool named 'raid' from myfreenas.example.com by using 'myuser'
  user, 'mqlkdqfmLIHMOyçè67sdf' authentication password, 'yYOOJMohimoç96e283'
  private password, 'SHA' authentication protocol, 'AES' privacy protocol,
  and raise warnings if 'raid' zpool status is greater than "ONLINE"
  (0) and critical if greater than "DEGRADED" (1).

## Debugging:
  -vv option will display the executed method while running the plugin

  -vvv option will also display all the SNMP dialog managed by Net::SNMP

## LICENSE AND COPYRIGHT

Copyright (C) 2018 Thomas Cazali

This program is distributed under the (Simplified) BSD License:
[http://www.opensource.org/licenses/BSD-2-Clause](http://www.opensource.org/licenses/BSD-2-Clause)

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:

* Redistributions of source code must retain the above copyright
notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright
notice, this list of conditions and the following disclaimer in the
documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
