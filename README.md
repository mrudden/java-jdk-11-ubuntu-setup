# java-jdk-11-ubuntu-setup
What I needed to do to setup the Java JDK on Ubuntu (currently running in WSL2 on Windows 10 x64)

1. Browse to https://www.oracle.com/java/technologies/javase-jdk11-downloads.html.
2. Locate the "Linux x64 Debian Package" and click on the link to download jdk-11.0.9_linux-x64_bin.deb (latest JDK LTS at time of writing).
3. Create or sign in with an Oracle account.
4. Download the .deb file to a folder in your Windows username home directory. I created a folder called ".wsl-stash" for this and will use that in the examples below.
5. Open up Ubuntu and browse to the folder above.  
  I was able to use `cd /mnt/c/Users/my_username/.wsl-stash` because the folder is on my C: drive.
6. Run `dpkg -i jdk-11.0.9_linux-x64_bin.deb` to install the JDK on Ubuntu.
7. Next, I tried to run `java` to test the install. This did not work. Instead of getting anything from the Java program, I got this helpful output from Ubuntu:
  ```
  me@computername:/mnt/c/Users/my_username/.wsl-stash$ java

  Command 'java' not found, but can be installed with:

  sudo apt install openjdk-11-jre-headless  # version 11.0.9.1+1-0ubuntu1~20.04, or
  sudo apt install default-jre              # version 2:1.11-72
  sudo apt install openjdk-8-jre-headless   # version 8u275-b01-0ubuntu1~20.04
  sudo apt install openjdk-13-jre-headless  # version 13.0.4+8-1~20.04
  sudo apt install openjdk-14-jre-headless  # version 14.0.2+12-1~20.04
  ```
  - After some searching on DuckDuckGo, I determined I needed to set links in Ubuntu since it is expecting the average user to just install the openjdk. I found [this helpful website](https://websiteforstudents.com/how-to-install-oracle-java-jdk-11-on-ubuntu-18-04-16-04-18-10/) that put me on the right path.

8. To get the version of Java linked to where Ubuntu expects it, you need to run these commands:
  ```
  sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-11.0.9/bin/java 1
  ```
  ```
  sudo update-alternatives --config java
  ```

9. Now test again by running `java --version`. If you get the following output, you're in a good place:
  ```
  java 11.0.9 2020-10-20 LTS
  Java(TM) SE Runtime Environment 18.9 (build 11.0.9+7-LTS)
  Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.9+7-LTS, mixed mode)
  ```
10. Next up I need to link up the binaries (executable programs) within the "/usr/lib/jvm/jdk-11.0.9/bin/" folder so that I can run other built-in programs from the command line. Following instructions from [the link above](https://websiteforstudents.com/how-to-install-oracle-java-jdk-11-on-ubuntu-18-04-16-04-18-10/) and adding javadocs because I know I'll need that, that gave me the following commands:
  ```
  sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk-11.0.9/bin/jar 1
  ```
  ```
  sudo update-alternatives --set jar /usr/lib/jvm/jdk-11.0.9/bin/jar
  ```
  ```
  sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-11.0.9/bin/javac 1
  ```
  ```
  sudo update-alternatives --set javac /usr/lib/jvm/jdk-11.0.9/bin/javac
  ```
  ```
  sudo update-alternatives --install /usr/bin/javadoc javadoc /usr/lib/jvm/jdk-11.0.9/bin/javadoc 1
  ```
  ```
  sudo update-alternatives --set javadoc /usr/lib/jvm/jdk-11.0.9/bin/javadoc
  ```
- **NOTE:** This will need to be completed for any other binaries in the "/usr/lib/jvm/jdk-11.0.9/bin/" folder that you need to use!
11. Run any or all of those commands above (`jar`, `javac`, and/or `javadoc`) to test them out. If they still don't work, you may want to double check your paths first and do some searching for answers second.

## Further reading
- [What exactly does `update-alternatives` do?](https://askubuntu.com/questions/233190/what-exactly-does-update-alternatives-do)
- [Ubuntu update-alternatives manpages](https://manpages.ubuntu.com/manpages/focal/en/man1/update-alternatives.1.html)

## FAQ
- Q: Why didn't I just install openjdk with apt-get?

  - A: It's for a class that's structured around the Oracle JDK. We were provided instructions for Windows and Mac, and I wanted to use [Ubuntu on WSL2](https://ubuntu.com/blog/ubuntu-on-wsl-2-is-generally-available) in the new [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/) instead.

    I didn't want to accidentally introduce incompatibilities with the course content by using a different distribution of the JDK. That could be hard to troubleshoot if any issues arose.

- Q: How can I learn more about setting up WSL2?

  - A: Check out Microsoft's website - [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
  
- Q: Where'd that raw_history.txt file come from?

  - A: I ran `history` on the Ubuntu command line and copied and pasted some of the output into that file.
