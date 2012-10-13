JBoss as RPM build instructions
===============================

This project was inspired by http://code.google.com/p/jboss-rpm and modified
for my needs.  Startup scripts are managed manually, as the startup scripts in
the jboss distribution do not have the chkconfig comments.

The binaries are downloaded from maven.  I used JBoss 6.1.0.Final, but it may be
simple to change the version by updating that property in the pom.xml.  I didn't
try it.

The rpm is built for x86_64, since there are 64 bit hornetq libraries in the the
src/jboss-6.1.0.Final/bin/native directory after the jboss zip is downloaded.  If
you need to build a 32 bit version, you can try changing the <needarch> tag and
removing those files.  Maven shouldn't download the jboss zip if it already present.

Prerequisites to build the RPM
==============================

1) You must have maven installed and configured.  I used version 3.0.3

2) You must have rpmbuild tools installed.  Try 'sudo yum install rpm-build'

3) You must have a ~/.rpmmacros file in you $HOME directory.  A sample of this
   file is provided in rpmmacros.sample

Steps to build the RPM
======================

1) Clone the repo and cd into the root directory

2) Ensure you have prereqs

3) Run the following command

     mvn clean install

4) This will put the file in your ~/.m2/repository and

Steps to install the RPM
========================

1) Build the rpm with maven.  Beside the ~/.m2 directory, the rpm will also be located at
   target/rpm/jboss-as-rpm-6.1.0.Final/RPMS/x86_64/jboss-as-rpm-6.1.0.Final-1.0-1.x86_64.rpm

   See the pom for an explanation on the duplicate version in the path.

2) Move the file jboss-as-rpm-6.1.0.Final-1.0-1.x86_64.rpm to the server you want
   to install it on and change to that directory

3) Run the following

     sudo rpm -i jboss-as-rpm-6.1.0.Final-1.0-1.x86_64.rpm

4) Get a sandwich, because you just saved yourself at least an hour

Steps to uninstall the RPM
==========================

1) Change to the directory where the rpm file is located, from step 2 in the install directions

2) Use the rpm command to uninstall, ie

     sudo rpm -e jboss-as-rpm-6.1.0.Final-1.0-1.x86_64

   If you use tab complete fill in the filename, be sure to remove the .rpm

3) Decide if you want to keep the jboss user's home directory.  The jboss user and group will
   have been deleted, along with the jboss server.  I left the home directory just in case.
