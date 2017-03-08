# Introduction

This document describes some of the design decisions and technical details involved in running a CTF competition. It attempts to summarize some opinions held by the CTF community and list some specific pitfalls to avoid when designing problems. Comments or suggestsions? Pull requests welcome!

[The Many Maxims of Maximally Effective CTFs](http://captf.com/maxims.html) is another great resource with advice on how to get the most fun out of a CTF.

# General Design

It's a good idea to play plenty of CTFs so that you can know your target audience and be familiar with some of the decisions you will have to make. Remember that preparing and running a CTF always takes far more time and effort than expected, so make sure to budget plenty of time to work with :-).

## Timing

Try to use something like [ctftime.org](https://ctftime.org/) to avoid scheduling conflicts, mainly with other CTFs or security conferences. Plan ahead and announce your event early to give everyone else time to plan around it. You may further wish to look up historical dates for the larger competitions - they tend to happen around the same time every year--often tied to annual conferences--and usually can't reschedule to accommodate your CTF.

Try not to schedule your CTF during the middle of the week. This often makes playing harder for students and people with jobs.

Try to keep your competition open 24 or 48 hours: this gives teams around the world an equal amount of "daylight" hours to spend on things. It's usually a bad idea to keep things open for more than 2 days: either you can't actually fill up that much time with good problems; or you can, and competitors quickly get overwhelmed.

## Flag Format

While the flag format seems like a small detail, annoying flag formats can turn an otherwise decent CTF challenge into a frustrating experience.

Wherever possible, make the flag a simple ASCII string that competitors discover while solving the problem, and NOT a complex format like

`<md5(name of target)>_<date of event>_<function address>`

These formats are easy to accidentally under-specify! How should the name be capitalized? Does the date use slashes, dashes, or dots as separators? Does the date include the time, and in what time zone? How precise does the time need to be, seconds, milliseconds or microseconds? Is the function address in hex? Is it zero-padded to 8 or 16 digits? Does it start with "0x", or not?

Furthermore, compound flag formats often make it hard for competitors to know when they've found the right thing; there are always several permutations to try each time someone has a new guess, and it's not rewarding to fiddle with formats while wondering how close you are.

(There may be some good problems that absolutely require complex formats. Proceed with caution, and watch out for ambiguity.)

Do not ask players to hash something they get and send the hash as the flag. As with the above, it is difficult to specify exactly what needs to go into the hash, and trivial errors in capitalization or newlines will give an incorrect result. Additionally, the hash itself will not be harder to guess than whatever the original flag was, so why not just submit that directly? As an organizer, it is also useful to watch submission logs to see what people are sending in as flags to get an idea of whether or not hints are needed or players are hitting unexpected roadblocks. If all you see are a slew of MD5 hashes, you won't have a clue what is going on.

A flag should always look obviously like a flag to submit. If the flag
is "Congratulations you win!" or "1234", some competitors may not
realize when they have solved the challenge and continue to spend time
on it. It's recommended that organizers pick a common format like
"MyCTF{663d63e8c755f1b4}" or "funny\_1337speaK\_pHras3" for ALL flags in a competition. (It also helps if you can include "The flag is:" in front of some flags).

Avoid brute forcible flags. For example, if flag has to be the name of a city, some players might try to submit thousands of guesses. Please do not put a CAPTCHA in front of all key submissions. This is unnecessary and extremely annoying to players.

Be maximally permissible with flag checking. The flag submission should be case-insensitive and reasonably flexible. A nice feature is to accept any of "CTF{663d63e8c755f1b4}", "663D63E8C755F1B4", "flag is: 663D63E8C755F1B4", etc. Another nice feature is to trim/strip white space characters in flag submissions as they can easily be added when copying and pasting into web forms.

## Mechanics

CTFs are games for fun, but are also largely security skill exercises. Gameplay mechanics surrounding the challenges can add fun and intrigue, but don't always exercise security skills.  The best CTF challenges are both fun and on-topic. Having teams strategize about game mechanics isn't bad, but the core of a CTF is always its security challenges.

Therefore, any CTF scoring system--however complex--must strongly tend to reward the teams that demonstrate the best security skills. Think hard on your game mechanics, and try to keep them from being exploitable or distracting too much from the core challenges. (Not that that can't be fun, but little in this document would apply to such a competition!)

Some common mechanics for Jeopardy-style CTFs:

1. Make the more difficult "tour-de-force" problems worth more points, and the easier or higher-variance trivia and guessing problems worth fewer points. This encourages people to look at the difficult problems and learn hardcore security skillz! Don't worry about fine-tuning point values: there is no "perfect". Keep in mind that people who test a problem typically know its difficulty better than the problem author, so testers usually assign the most suitable point-values.
2. Employ "breakthru points": extra points (usually only good for tie-breaking) given to the first three-or-so teams to solve each particular challenge. This discourages "flag hoarding", where a team holds on to their flags and only submits them all at the very end of the competition (which is often advantageous for that team). It's demoralizing to find out that you are not doing as well as you thought, so it is useful to have anti-hoarding measures like breakthru points.
3. The open/closed status of a problem should be global, meaning that if a problem is open for one team, it should be open for all teams. This gives all teams a fair time and chance to look at all of the problems, and avoids situations where a team gets screwed by opening unlucky challenges.
4. Ensure that all teams on the scoreboard always have at least 2-3 problems open to work on. It's all too easy for any team to get stuck on some particular challenge (possibly through no fault of their own!), and making sure that all players have something that they'd like to work on in their hands is the best way to keep everyone happy.
5. Limit the number of unsolved challenges opened. This helps to avoid giving large teams a huge advantage. And hey, you just might be able to save some unopened challenges for next year ;)

## Testing

Testing is the difference between a horrible problem and a great one. Have a reference solution you can run to verify services are working and solvable whenever someone asks. Have someone other than the original problem author write the solution to make sure it's doable without having to be a psychic. If you don't have time to test everything, focus on the "black box" problems first.

## Communication

Organizers should strive to be reachable throughout the entire CTF. Have an IRC channel and monitor it throughout the CTF. Give organizers operator status on the channel so that players know who they can discuss problem details with.

Publish and monitor an email address.

Have at least one other communication channel like a Twitter stream or a news page on the site to ensure that nobody misses any important game updates.

## Problem Updates

Whenever a change is made to a problem, announce it in a permanent and visible place, in addition to in IRC. Make sure to update the problem description to reflect the change. For any downloadable problem files which have changed, update the file name so that it is obvious it has changed.

When applicable, leave the original version of the problem available and running. For example, someone might be really close on that pwnable you want to update and make easier :)

If a problem has already been solved, be extremely sensitive about making changes to problems (this includes changing the point value, problem files, description, hints, etc.). These issues usually need to be handled case-by-case, so please exercise good judgement!

## Problem Distribution

The easiest way to distribute files is via HTTP from your scoreboard server. Try to avoid using services like Dropbox and Google Drive, as you may hit bandwidth limits on those.

If you are super prepared and have large files, the best way to distribute them is to symmetrically encrypt them with a free tool like gpg or openssl and distribute a torrent before the CTF. You can then release the encryption passphrase at the start (or in the middle) of the CTF to give people access to the file.

Make sure that the files for problems are not accessible until the problem is opened. One simple way to do this is to append a hash of the file contents to the filename (so that filenames are not guessable) and disabling directory indexes on your webserver.

It is also nice if problems do not require logging in to download them. This allows players to easily work from the command line with tools like wget and curl, without having to set up obnoxious cookies or setup scripts to handle logging in. As long as your filenames are secret, there should be no problem with this.

## Infrastructure

Like everything else, test all parts of the infrastructure carefully. Many CTF organizers have found it useful to host their website and problems on the cloud. This makes it quick and easy to spin up more instances as needed.

Make sure to do proper testing on the final production infrastructure! For example, in the first pCTF, the pwnables were running on a machine which didn't support NX. This was missed until after the CTF because the production infrastructure was not thoroughly tested.

A partial checklist list of things to test on the production infrastructure:

* End to end testing of team registration and key submission
* Ensure key submission does not have double-counting race conditions.
* Perform load tests on scoreboard and key submission.
* Check that pwnable machines support the desired protections.
* Test full solutions for all problems - this means that you can run your solution script and it outputs the correct key. "My solution script gets eip=0x41414141" may not be enough!
* Test out the process for updating a problem during the CTF - mistakes happen, so it's best to be prepared.
* Test that the binaries/code running on your services match any files that you are giving out.

# Problems

Remember that the goal of a CTF is for the players to learn and have fun! The point of a problem is to be solved, so it's nice when every problem is solved by at least one team. Be creative and try to make sure solvers learn something cool from your problems - remember that the players are your customers, so try to make them happy :-) There are some more specific recommendations for certain categories of problems below:

## Pwnables

These pwnables sections only specifically cover Linux binaries.

### Local

Local pwnables usually involve SSHing into a machine and exploiting a setuid/setgid binary there. A great way to do this is to create per-team accounts on the machine so that teams can work without interfering with or leaking info to each other. Here is a short checklist of things to configure on the machine:

* Make sure your machine is fully patched and up-to-date.
* Prevent forkbombing or other resource exhaustion via limits.conf
* sysctl -w kernel.dmesg_restrict=1 # and set persist it in /etc/sysctl.conf
* mount -o remount,hidepid=2 /proc # prevent users from seeing each other's processes
* chmod 1733 /tmp /var/tmp /dev/shm # to avoid people leaking work to others # If you have per-user home dirs, then you can just chmod it 700 :-)
* Create a user for the problem, put the problem at /home/problemuser/problem
* chown -R root:root /home/problemuser
* chown root:problemuser /home/problemuser/problem
* chmod 2755 /home/problemuser/problem
* touch /home/problemuser/flag
* chown root:problemuser /home/problemuser/flag
* chmod 440 /home/problemuser/flag

As with all problems, make sure to fully test it after it is fully setup. Specifically, you'll want to make sure that your reference solution works as one of the CTF users and that the flag isn't readable via any other means, or writeable by any user besides root.

### Local Kernel

Local kernel exploitation challenges typically involve SSHing into a machine and exploiting a custom kernel driver. This type of challenge can be difficult to reliably host, and are not easily scalable. Since failed exploitation typically brings down the OS, each team should have their own isolated VM. Kernel challenges may be more appropriate for CTF "finals" where the number of teams is small and sufficient system resources may be allocated.

A possible setup could be running one or many ESXi hosts with a separate VM designated to each team. Provide SSH credentials to each team.

A few tips and reminders:
 * Instead of making 20 separate VMs, create one base VM and create 20 linked clones.
 * Log in after each VM is created and configure a unique static IP address.
 * Ensure that the OS is completely updated and patched against all public vulnerabilities.
 * REMOVE THE USER FROM SUDO ACCESS!
 * Create the flag in /root and: chmod 400 -R /root; chown root:root -R /root
 * Drop a staff member SSH key in /root/.ssh/ and enable remote root SSH login to help troubleshoot any potential issues.
 * Permit all users to read /proc/kallsyms unless an info leak is part of the challenge: echo 0 > /proc/sys/kernel/kptr_restrict
 * Disable kernel panics on oopses: echo 0 > /proc/sys/kernel/panic_on_oops
 * Develop a simple (authenticated) script for teams to call that reboots the remote VM. This should interface with the hypervisor, not the guest since the guest OS may be unresponsive after an exploitation attempt. Otherwise you will have teams frequently asking for a reboot.
 * Ensure that a working solution exists and that all expected mitigations are actually functional in the VM. This includes the presence of enforced read-only memory, SMEP/SMAP, etc.

Kernel challenges should be fun! Don't just install an old OS and task competitors with compiling public exploits. There are many ways to be creative with your challenge!

Alternatively, it is simple to build a very minimal VM with [buildroot](http://buildroot.uclibc.org/), which can be launched via qemu on connection. The above notes apply, but there are a few other caveats with this approach.

A few tips:
 * Ensure that the vm has networking, pasting hex/shellcode into some super-minimal vm is not fun.
 * Disable access to the qemu monitor `-monitor /dev/null` to disable debugging of the running vm.
 * If you can, make the login shell the qemu instance, with `curses` mode, to allow for Ctrl+C without killing the connection.
 * If this is a large game, it may make sense to place some sort of captcha/proof of work in front of spinning up qemu.
 * Ensure that every connection gets a "fresh" vm, replacing the disk image with a copy on connect is easy, and in the case of a small vm, shouldn't have much overhead.
 * KVM can't be used in a virtualized environment like EC2/Azure/etc. Test your VMs speed without hardware virtualization to make sure that it is responsive enough.


### Remote

Remote pwnables involve running a vulnerable network service. There are two popular ways to go about this, xinetd and doing fork/accept in the binary itself.

Do not use threads for each connection without fully understanding the consequences. This usually allows users to interfere with each other's exploits (accidentally or intentionally) and can make a problem extremely frustrating.

If your problem relies on leaking libc, consider providing the libc.so along with the problem binary. libc hunting isn't really an interesting skill to test in a CTF.

If you are using your own fork/accept server instead of xinetd, you should take special care to make sure that somebody who exploits the service cannot kill or take over it. The normal way to do this is to start the service as root and drop privileges after forking (and make sure not to leak that socket fd).

See [fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c) for a sample fork/accept server following this recommendation.

See [example.xinetd](https://github.com/pwning/docs/blob/master/example.xinetd) for a sample xinetd config for an xinetd service.

If you decide to run your challenge in a chroot or restricted environment, make sure that it has basic programs like /bin/sh, /bin/bash, /bin/cat, etc. If this is not possible, then make that clear in the problem description. It is extremely frustrating to fully exploit a service and then waste an hour before realizing that it is being run in a limited chroot.

Setup instructions for a remote pwnable:

* Create a user for the problem, put the problem at /home/problemuser/problem
* chown -R root:problemuser /home/problemuser
* chmod 750 /home/problemuser
* touch /home/problemuser/flag
* chown root:problemuser /home/problemuser/flag
* chmod 440 /home/problemuser/flag

Avoid relying on short reads. These can be extremely painful to get right remotely. Instead, consider reading one byte at a time until a delimiter or reading length-delimited strings (e.g. read 4 bytes little endian length followed by length bytes of data). Similarly, make sure to check the return value on calls to read/recv to make sure you're not dropping user input.

Just to be clear, here's an example of the wrong way to read 4096 bytes:

```c
char buf[4096];
recv(fd, buf, sizeof(buf));  // This is wrong, recv might return <4096
```

For a better way, see the `recvlen` function in [fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c).

Place the flag in a predictable location such as /home/problemuser/flag. It is frustrating to waste time hunting for the flag file after you have successfully exploited a service.

### General notes

One of the most important parts of making a working pwnable is proper testing (ideally have it tested by at least one person other than the author). Any time somebody complains that a pwnable is not working, you should have a full reference solution against the live instance that you can run to verify whether the problem is working or not.

Here are some more general annoying things in pwnables:

* Obnoxious output parsing. Please keep the output simple and reasonable to parse. The best type of output to parse is a length delimited string.

  Example of an obnoxious output format:
  ASCII decimal length delimited strings: 121A1B1C1D1E1F
  How are we supposed to parse this? Is it (12, '1A1B1C1D1E1F') or it is (1, '2'), (1, 'A'), (1, 'B'), (1, 'C'), (1, 'D'), (1, 'E'), (1, 'F')?

  Another obnoxious output format is stuff with ANSI escape codes. Have some self control, please :-)
* Even though the binary shows that NX is enabled, the machine it's running on doesn't support it.
* Nonsensical code and "fake" bugs. If 90% of your code is just checking inputs against a bunch of random constants to waste the reverser's time, it is probably not a very fun problem. If your bug is "the program jumps into this buffer for no good reason when these random constraints are satisfied" then you should probably push yourself to be a little more creative :-)

### Compile time protections

Often, a pwnable requires a specific set of protections to be enabled. Here's how to force them on / off in `gcc`

* `-fstack-protector` / `-fno-stack-protector`: Stack canaries
* `-D_FORTIFY_SOURCE=2` / `-D_FORTIFY_SOURCE=0` (prepend `-U_FORTIFY_SOURCE` to silence re-definition warnings): Use "`*_check`" versions of libc functions like `memcpy()`, `sprintf()`, `read()`, etc. that abort when they detect buffer overflows. (Note that detection is far from perfect and does not work in many cases.)
* `-fPIE -pie` / `-fno-PIE`: Position independent code (extends ASLR to also randomize the main binary, not just libraries). Note that PIE is usually ineffective on 32 bit, i.e. a PIE-unaware exploit will land once every couple hundred/thousand times. `-fPIC` is a version of `-fPIE` that doesn't require the resulting code to be part of the main executable (by avoiding certain optimizations), and there's also `-fpie` and `-fpic` which are dumb.
* `-Wl,-z,relro,-z,now` / `?`: Full RELRO (the GOT and PLT will be written and mapped read-only during program load).

As security becomes a more mainstream issue (yay!), more compile-time and runtime protections are being enabled by default. So, to avoid surprises you should really test your problems in their final configuration & setup.

## Web Challenges

If your challenge requires a large number of requests or measuring timing, ensure that it is still reasonable to solve remotely. Better yet, have a shell server on the same network (perhaps a pwnable with an SSH login) with common scripting languages and libraries installed where players can run their attacks from.

Here are some common things to avoid:

* Requiring players to guess URL parameters out of thin air (like ?debug=1)
* Requiring players to guess file or directory names (or dos your server with dirbuster)
* Requiring players to guess credentials

(Notice a pattern here?)

One great type of web problem is one where full source is given (and despite this it is still challenging).

## Reversing

It is very important to ensure that problems which check whether an input is valid always accept exactly one solution. This is by far the most common problem-breaking mistake in reversing challenges. If this is not possible to do, then make this clear in the problem description and have a form which accepts any input flag and outputs a flag that can be submitted for points.

Although a common real world use of reverse engineering is malware, it is generally considered to be in bad taste to distribute malicious reverse engineering challenges. If you are going to do this, be sure to clearly state that the programs are malicious. 

## Crypto

In general try to give the competitors as much information as you can. *Working* source code with secret keys clearly X'ed out is ideal.

If you have a ciphertext only problem, try to do the following:

* Give enough ciphertext for meaningful statistics (twenty ASCII characters can be almost anything)
* Use a guessable algorithm. With a classic cipher and a short amount of ciphertext, it may be very difficult to narrow it down. The challenge should be in breaking the crypto system, not figuring out what the crypto system is.
* If you are using any crypto system made after the 19th century, be sure to tell challengers what the algorithm is. No one wants to guess whether ciphertext is from an Enigma or a Purple Machine; or from 3DES or GOST

If your problem requires a lot of local computation, make sure to test it on reasonable consumer hardware. Realizing late in the competition that while you have the right method of solving the probem, you don't have the computation power to complete the computation before the end is a bad feeling. In general, under an hour on fairly modern consumer hardware is fine; more than that should have good justification.

## Forensics

## Miscellaneous

Try to avoid these as much as possible:

* Random guessing challenges
* Cracking passwords on zip files or stego programs
* Steganography problems
* Anything that is solved by just running metasploit, nessus, dirbuster, etc. Good CTF problems should require skill.
* Time-consuming recon challenges.
