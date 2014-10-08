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

Be maximally permissible with flag checking. The flag submission should be case-insensitive and reasonably flexible. A nice feature is to accept any of "CTF{663d63e8c755f1b4}", "663D63E8C755F1B4", "flag is: 663D63E8C755F1B4", etc.

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

# Problems

Remember that the goal of a CTF is for the players to learn and have fun! The point of a problem is to be solved, so it's nice when every problem is solved by at least one team. Be creative and try to make sure solvers learn something cool from your problems - remember that the players are your customers, so try to make them happy :-) There are some more specific recommendations for certain categories of problems below:

## Pwnables

The below two sections are specific to pwnables on Linux:

### Local

Local pwnables usually involve SSHing into a machine and exploiting a setuid/setgid binary there. A great way to do this is to create per-team accounts on the machine so that teams can work without interfering with or leaking info to each other. Here is a short checklist of things to configure on the machine:

* Make sure your machine is fully patched and up-to-date.
* Prevent forkbombing or other resource exhaustion via limits.conf
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

### Remote

Remote pwnables involve running a vulnerable network service. There are two popular ways to go about this, xinetd and doing fork/accept in the binary itself.

Do not use threads for each connection without fully understanding the consequences. This usually allows users to interfere with each other's exploits (accidentally or intentionally) and can make a problem extremely frustrating.

If your problem relies on leaking libc, consider providing the libc.so along with the problem binary. libc hunting isn't really an interesting skill to test in a CTF.

If you are using your own fork/accept server instead of xinetd, you should take special care to make sure that somebody who exploits the service cannot kill or take over it. The normal way to do this is to start the service as root and drop privileges after forking (and make sure not to leak that socket fd).

See Appendix A for a sample fork/accept server following this recommendation.

See Appendix B for a sample xinetd config for an xinetd service.

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
recv(fd, buf, sizeof buf);  // This is wrong, recv might return <4096
```

For a better way, see the `recvlen` function in Appendix A.

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
* If you are using any crypto system made after the 19th century, be sure to tell challengers what the algorithm is. No one wants to guess whether ciphertext is from an Enigma or a Purple Machine; or from 3DES or GHOST

## Forensics

## Miscellaneous

Try to avoid these as much as possible:

* Random guessing challenges
* Cracking passwords on zip files or stego programs
* Steganography problems
* Anything that is solved by just running metasploit, nessus, dirbuster, etc. Good CTF problems should require skill.
* Time-consuming recon challenges.

# Appendix A

A sample forking/listening CTF binary (shamelessly reverse engineered from an old Ghost in the Shellcode challenge). You don't have to use this exact code, but make sure that your forking/listening servers are not susceptible to the problems mentioned in the comments.

```c
#include <arpa/inet.h>
#include <errno.h>
#include <pwd.h>
#include <signal.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/types.h>  
#include <unistd.h>

const uint16_t port = 1234;

// Remember to check return values carefully in this function.
// Don't want to accidentally give people root :-)
int drop_privs(char *username) {
    struct passwd *pw = getpwnam(username);
    if (pw == NULL) {
        fprintf(stderr, "User %s not found\n", username);
        return 1;
    }

    if (chdir(pw->pw_dir) != 0) {
        perror("chdir");
        return 1;
    }

    // Don't forget to drop supplemental groups. Forgetting this
    // has led to people escalating to root in some past CTFs :-)
    if (setgroups(0, NULL) != 0) {
        perror("setgroups");
        return 1;
    }

    if (setgid(pw->pw_gid) != 0) {
        perror("setgid");
        return 1;
    }

    if (setuid(pw->pw_uid) != 0) {
        perror("setuid");
        return 1;
    }

    return 0;
}

// It is recommended to have wrappers like this around send and recv.
// Remember that send/recv can return without reading all n requested
// bytes.
ssize_t recvlen(int fd, char *buf, size_t n) {
    ssize_t rc;
    size_t nread = 0;
    while (nread < n) {
        rc = recv(fd, buf + nread, n - nread, 0);
        if (rc == -1) {
            if (errno == EAGAIN || errno == EINTR) {
                continue;
            }
            return -1;
        }
        if (rc == 0) {
            break;
        }
        nread += rc;
    }
    return nread;
}

ssize_t sendlen(int fd, const char *buf, size_t n) {
    ssize_t rc;
    size_t nsent = 0;
    while (nsent < n) {
        rc = send(fd, buf + nsent, n - nsent, 0);
        if (rc == -1) {
            if (errno == EAGAIN || errno == EINTR) {
                continue;
            }
            return -1;
        }
        nsent += rc;
    }
    return nsent;
}

ssize_t sendstr(int fd, const char *str) {
    return sendlen(fd, str, strlen(str));
}

// The connection handling function.
// Put your vulnerable code here :-)
int handle(int fd) {
    char buf[64];
    size_t len;
    recvlen(fd, (char *) &len, sizeof(len));
    recvlen(fd, buf, len);
    sendstr(fd, buf);
}

int main(int argc, char **argv) {
    int rc;
    int opt;
    int sockfd;
    int clientfd;
    pid_t pid;
    struct sockaddr_in saddr = {0};
    // Setting the SIGCHLD handler to SIG_IGN prevents child
    // processes from becoming zombies (so you do not need to
    // call wait() on them).
    if (signal(SIGCHLD, SIG_IGN) == SIG_ERR) {
        fputs("Failed to set SIGCHLD handler.", stderr);
        return 1;
    }

    sockfd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    if (sockfd == -1) {
        perror("socket");
        return 1;
    }

    // Set SO_REUSEADDR. Otherwise, if the server crashes for
    // any reason, you will have to wait for sockets to time
    // out before you can reuse the port.
    opt = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &opt,
                   sizeof(opt)) != 0) {
        perror("setsockopt");
        return 1;
    }

    saddr.sin_family = AF_INET;
    saddr.sin_addr.s_addr = htonl(INADDR_ANY);
    saddr.sin_port = htons(port);

    if (bind(sockfd, (struct sockaddr *) &saddr,
             sizeof(saddr)) != 0) {
        perror("bind");
        return 1;
    }

    if (listen(sockfd, 20) != 0) {
        perror("listen");
        return 1;
    }

    while (1) {
        clientfd = accept(sockfd, NULL, NULL);
        if (clientfd == -1) {
            perror("accept");
            continue;
        }

        pid = fork();
        if (pid == -1) {
            perror("fork");
            close(clientfd);
            continue;
        }

        if (pid == 0) {
            // Avoid tons of long-running processes sticking around.
            alarm(15);

            // If you do not close the socket fd, someone who
            // exploits the service could call accept() on it and
            // hijack other people's connections.
            close(sockfd);

            // The server is started as root and drops privileges
            // after forking and before handling the request.
            // Otherwise, someone who exploits the service can
            // kill, ptrace, or otherwise interfere with the server.
            rc = drop_privs("problemuser");
            if (rc == 0) {
                rc = handle(clientfd);
            }

            close(clientfd);
            _exit(rc);
        }

        // If you forget to close the client fd,  you could run
        // out of file descriptors (it also makes the connection fd
        // hard to predict, which can be annoying for someone
        // writing an exploit - if you want to do this on purpose,
        // use dup2 with a random fd instead :-P).
        close(clientfd);
    }

    return 0;
}
```

# Appendix B

A sample xinetd config for an xinetd CTF binary. For rlimits / chdir / other setup, it may be convenient to point xinetd at a wrapper shell script which performs the setup and then exec the binary.

```
service problem
{
    disable = no
    socket_type = stream
    protocol    = tcp
    wait        = no
    user        = problemuser
    bind        = 0.0.0.0
    server      = /home/problemuser/problem
    type        = UNLISTED
    port        = 9999
}
```
