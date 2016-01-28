---
title: Recover BGP password
permalink: /Recover_BGP_password/
---

**\# Get a root shell:**

user@router&gt; start shell

% su - root

Password:

root@router%

**\# View the contents of /var/etc/keyadmin.conf**

root@router% less /var/etc/keyadmin.conf

tcp 179 0.0.0.0 10.0.0.1 md5 instance default 0x6162636431323334

tcp 179 :: 2001:DB8:1::1 md5 instance default 0x313233717765727479

**\# Run the following command on a system with Perl :**

user@box:~&gt;perl -e 'print "Hex: ";$_=&lt;&gt;;print "MD5: ";s/(\\w\\w)/\\1:/g;for (split(/:/)) {printf "%s", chr(hex($_))};print "\\n"'

Hex: 0x6162636431323334

MD5: abcd1234

user@box:~&gt;perl -e 'print "Hex: ";$_=&lt;&gt;;print "MD5: ";s/(\\w\\w)/\\1:/g;for (split(/:/)) {printf "%s", chr(hex($_))};print "\\n"'

Hex: 0x313233717765727479

MD5: 123qwerty

If you'd like to do this in more of a 'batch' method, you can adapt this perl script to simply be a paramater so you can iterate through your keyadmin.conf and retrieve all passwords.

` #!/usr/bin/perl`
` `
` $_=$ARGV[0];`
` `
` s/(\w\w)/\1:/g;`
` `
` for (split(/:/)) {`
`       printf "%s", chr(hex($_))`
` };`
` `
` print "\n";`

Example:

` ./junospassword.pl 0x313233717765727479`
` 123qwerty`

That's it! Many thanks for the person who provided me this code and to the person who developed it :)

Alternatively, you can use the following code to convert your juniper passwords to Cisco configuration commands:

` #!/usr/bin/perl    `
` `
` while (<>) {`
`         next unless (/^tcp 179.*\s+(\S+)\s+md5\s+instance\s+default\s+0x([0-9a-f]+)\s*$/);`
`         my ($addr, $password) = ($1, $2);`
` `
`         $password =~ s/([a-fA-F0-9][a-fA-F0-9])/chr(hex($1))/eg;`
`         print "neighbor $addr password $password\n";`
` }`

To use this code, download the contents of /var/etc/keyadmin.conf to your local unix machine and execute:

` % perl convert-junos-password.pl < keyadmin.conf`
` neighbor 10.10.22.22 password s00pers3kr1t`
` neighbor 172.16.99.99 password 3v3nm0aRs3kr1t`
` %`

Alternatives
------------

See \[<http://search.cpan.org/perldoc?Crypt>::Juniper Crypt::Juniper\] perl module.

Go to [password-decrypt.com](http://password-decrypt.com) for an easy to use online tool.