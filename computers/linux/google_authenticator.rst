Google Authenticator
====================

*Work in progress*

**SELinux**

Google authenticator updates the .google_authenticator file. This is blocked by default SELinux rules. Of course, a common thing to do is to disable SELinux since the blocking it does is often a pain. I, however, do not want to disable it. Luckily, there is a fix. The audit tools suggest allowing sshd to write files of type user_home_dir_t. I thought this seemed like a bad idea, because if it didn't already have that permission, there was probably a reason. So what I did was use the fact that sshd could already access .ssh, and move the .google_authenticator file to .ssh/. Then I changed the type to ssh_home_t using the command

     $ *chcon -t ssh_home_t .google_authenticator.*

This fixed it without opening up user_home_dir_t to sshd.
