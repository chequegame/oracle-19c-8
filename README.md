# [ Oracle Complete Scenario ] GI 19c [ASM] DB 19c on [ Oracle Linux 8.6 ]

In this Document we will start with VirtualBox and Oracle Linux 8.6 installation, then move to apply the prerequisites for both GI and DB, after that we going to proceed to Oracle Grid Infrastructure 19c installation with ASM then Oracle 19.3 Database software only installation, Once we finish the installation, we will create database.

## Table of Contents

- [[ Oracle Complete Scenario ] GI 19c [ASM] DB 19c on [ Oracle Linux 8.6 ]](#-oracle-complete-scenario--gi-19c-asm-db-19c-on--oracle-linux-86-)
  - [**Requirements:*](#requirements)
  - [Installing VirtualBox.](#installing-virtualbox)
  - [Installing OEL 8.6 in VirtualBox.](#installing-oel-86-in-virtualbox)
    - [Creating Virtual Machine Instance.](#creating-virtual-machine-instance)
    - [Installing OEL 8.6](#installing-oel-86)
    - [Installing Guest Additions.](#installing-guest-additions)
  - [Prepare the OS for the Installation [Prerequisites]](#prepare-the-os-for-the-installation-prerequisites)
  - [Create and Tag ASM Disks.](#create-and-tag-asm-disks)
  - [Installing Oracle Grid Infrastructure 19c Software](#installing-oracle-grid-infrastructure-19c-software)
    - [Creating FRA Disk Group.](#creating-fra-disk-group)
  - [Installing Oracle DB 19c software Only](#installing-oracle-db-19c-software-only)
  - [Create database using dbca.](#create-database-using-dbca)
  - [Verifying the installation.](#verifying-the-installation)

## **Requirements:*

1. [Oracle Linux 8.6 ISO](https://yum.oracle.com/ISOS/OracleLinux/OL8/u6/x86_64/OracleLinux-R8-U6-x86_64-dvd.iso).
2. [ASMLib8](https://www.oracle.com/linux/downloads/linux-asmlib-v8-downloads.html) and [oracleasm-support](ttps://public-yum.oracle.com/repo/OracleLinux/OL8/addons/x86_64/getPackage/oracleasm-support-2.1.12-1.el8.x86_64.rpm).
3. [Oracle 19.3 Grid Infrastructure (GI)](https://www.oracle.com/database/technologies/oracle19c-linux-downloads.html#license-lightbox).
4. [Oracle 19.3 Database (DB)](https://www.oracle.com/database/technologies/oracle19c-linux-downloads.html#license-lightbox).
5. [VBox](https://www.virtualbox.org/wiki/Downloads) installed.
6. Virtual Machine Resource allocation:
   * 8 GB RAM (Minimum 4GB)
   * 2 Cores [CPU]
   * 80 GB Virtual Disk for OS
   * 8 x 5GB Virtual Disk for ASM



## Installing VirtualBox.

1. Search "Virtual Box" in Google and click in first Download option:  
    ![Virtual Box Google Search](assets/virtualbox/capture_18-09-22_17-47-36.png)
2. In Platform Packages select Windows Hosts  
    ![Download Section Virtual Box](assets/virtualbox/capture_18-09-22_17-47-48.png)
3. Download will start, save it in Desktop  
    ![Browser Downloads](assets/virtualbox/capture_18-09-22_17-47-52.png)  
    ![Windows Explorer](assets/virtualbox/capture_18-09-22_17-48-02.png)
4. Download finished  
    ![Virtual Box Exe in Desktop](assets/virtualbox/capture_18-09-22_17-48-13.png)
5. Run the Virtual Box Installer, click Next.  
    ![Virtual Box Installer 1](assets/virtualbox/capture_18-09-22_17-48-35.png)
6. Click next.  
    ![Virtual Box Installer 2](assets/virtualbox/capture_18-09-22_17-48-46.png)
7. Select only the last checkbox.  
    ![Virtual Box Installer 3](assets/virtualbox/capture_18-09-22_17-49-07.png)
8. Accept the warning (you will lost internet connection).  
    ![Virtual Box Installer 4](assets/virtualbox/capture_18-09-22_17-49-14.png)
9. Click in Install.  
    ![Virtual Box Installer 5](assets/virtualbox/capture_18-09-22_17-49-19.png)
10. The installation begins.  
    ![Virtual Box Installer 6](assets/virtualbox/capture_18-09-22_17-49-24.png)
11. The installation completes.  
    ![Virtual Box Installer Finished](assets/virtualbox/capture_18-09-22_17-49-40.png)
12.  Virtual Box is installed.  
    ![Virtual Box Installer 1](assets/virtualbox/capture_18-09-22_17-49-56.png)
  
  
## Installing OEL 8.6 in VirtualBox.
### Creating Virtual Machine Instance.

1. Click on New.
    ![New VM Virtual Box](assets/os/instance/capture_24-09-22_19-19-09.png)
2. Change the name to "OEL86" and click Next.
    ![Rename VM](assets/os/instance/capture_24-09-22_19-20-07.png)
3. Assign 8192 MB(8GB) of RAM. (Minimum 4096 MB(4GB)).
    ![Memory Size VM](assets/os/instance/capture_24-09-22_19-20-29.png)
4. Select "Create virtual hard disk now" and click Create.
    ![Create virtual disk](assets/os/instance/capture_24-09-22_19-20-42.png)
5. Select VDI (VirtualBox Disk Image)
    ![VM Disk File Type](assets/os/instance/capture_24-09-22_19-20-53.png)
6. Select Dynamically Allocated and click Next.
    ![VM dynamic virtual hard disk](assets/os/instance/capture_24-09-22_19-21-00.png)
7. Assign 80GB to the virtual file disk.
    ![Disk file size](assets/os/instance/capture_24-09-22_19-21-43.png)
8. Once created, click on Settings.
    ![VM Setting Icon](assets/os/instance/capture_24-09-22_19-22-04.png)
9. In General > Advanced, set both Shared Clipboard and Drag'n'Drop to Bidirectional.
    ![Bidirectional clipboard](assets/os/instance/capture_24-09-22_19-22-21.png)
10. In System > Processor, set 2 Processors.
    ![2 processors](assets/os/instance/capture_24-09-22_19-22-39.png)
11. In Storage > Controller: IDE, select the Empty disk.
    ![Empty IDE controller](assets/os/instance/capture_24-09-22_19-23-01.png)
12. Then select the disk icon to the right.
    ![disk icon](assets/os/instance/capture_24-09-22_19-23-12.png)
13. Select "Choose a disk file..."
    ![Choose disk file](assets/os/instance/capture_24-09-22_19-23-21.png)
14. Open the OEL86 ISO file.
    ![Windows explorer](assets/os/instance/capture_24-09-22_19-23-40.png)
15. ISO selected.
    ![ISO selected](assets/os/instance/capture_24-09-22_19-23-53.png)
16. Now in Controller: Sata select the icon with the plus symbol.
    ![Add SATA](assets/os/instance/capture_24-09-22_19-24-01.png)
17. Click on Create.
    ![Create SATA](assets/os/instance/capture_24-09-22_19-24-19.png)
18. Select VDI (VirtualBox Disk Image), click Next.
    ![Disk file type](assets/os/instance/capture_24-09-22_19-24-35.png)
19. Select Dynamically Allocated, click Next.
    ![Dynamic SATA Disk](assets/os/instance/capture_24-09-22_19-24-44.png)
20. Change de the disk name to ASMDisk*i* where *i*  will go from 1 to 8. Assign 5GB of size. Click on Create.
    ![Disk name and size](assets/os/instance/capture_24-09-22_19-25-10.png)
21. In the "No attached" section, select the just created virtual disk.
    ![Select created disk](assets/os/instance/capture_24-09-22_19-25-37.png)
22. Repeat steps 20 and 21 eight times.
23. 8 disks must be created.
    ![8 disks created](assets/os/instance/capture_24-09-22_19-29-38.png)
24. In Network > Adapter 1, select Bridged Adapter and the network interface in which you have internet access.
    ![Network Adapter 1](assets/os/instance/capture_24-09-22_19-30-07.png)
25.  In Adapter 2, use the same settings as the previous step. Cick on OK.
    ![Network Adapter 2](assets/os/instance/capture_24-09-22_19-30-29.png)
26. In the VM Instance, right click on it and select Clone.
    ![Clone Option](assets/os/instance/capture_24-09-22_19-31-02.png)
27. Append to the name "Null" and the Keep Disk Names checkbox. This clone will be useful just in case some thing fails during the OS installation.
    ![Clone name](assets/os/instance/capture_24-09-22_19-31-45.png)
28. Select Full Clone and click on Clone.
    ![Full Clone](assets/os/instance/capture_24-09-22_19-31-57.png)
29. Select the original instance and click on Start.
    ![Start original instance](assets/os/instance/capture_24-09-22_19-32-12.png)

### Installing OEL 8.6

1. Select with Intro, Install Oracle Linux 8.6.0
    ![](assets/os/oel86/capture_24-09-22_20-13-12.png)
2. The language must be English. Click on Continue.
    ![](assets/os/oel86/capture_24-09-22_20-15-56.png)
3. Click on Keyboard.
    ![](assets/os/oel86/capture_24-09-22_20-16-33.png)
4. Click on English (US) then the "-" Icon.
    ![](assets/os/oel86/capture_24-09-22_20-19-21.png)
5. Search for your keyboard distribution. In my case, Spanish Latin American. Click on Add.
    ![](assets/os/oel86/capture_24-09-22_20-20-20.png)
6. It should be just one distribution. Click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-20-40.png)
7. Click on Time & Date.
    ![](assets/os/oel86/capture_24-09-22_20-21-03.png)
8. Click on in the time zone you currenty are. In this case, Mexico City. Click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-21-17.png)
9. Click on Software Selection.
    ![](assets/os/oel86/capture_24-09-22_20-22-05.png)

10. Select Server with GUI as de Base Enviroment. Additiontal Software selection:  
    - Debugging Tools.
    - Performance Tools.
    - Remote Management For Linux.
    - Legacy Unix Compatibility.
    - Container Management.
    - Development Tools.
    - .NET Core Development.
    - Graphical Administration Tools.
    - RPM Development Tools.
    - Security Tools.
    - System Tools.  
  Click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-24-04.png)
11.  Click on Installation Destination.
    ![](assets/os/oel86/capture_24-09-22_20-24-31.png)
12. Select the 80GB hard disk. Check the Custom checkbox. Click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-24-45.png)
13. Click on the "+" Button.
    ![](assets/os/oel86/capture_24-09-22_20-25-02.png)
14. Select /boot as the Mount Point and 2 GiB as the Desired Capactity. Click on Add mount point.
    ![](assets/os/oel86/capture_24-09-22_20-25-30.png)
15. Click on the "+" Button.
    ![](assets/os/oel86/capture_24-09-22_20-27-47.png)
16.  Select swap as the Mount Point and 8 GiB as the Desired Capactity. Click on Add mount point.
    ![](assets/os/oel86/capture_24-09-22_20-28-13.png)
17. Click on the "+" Button.
    ![](assets/os/oel86/capture_24-09-22_20-28-28.png)
18.   Select / as the Mount Point and leave blank the Desired Capactity. Click on Add mount point.
    ![](assets/os/oel86/capture_24-09-22_20-28-42.png)
19. With the 3 partitions created. Click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-28-59.png)
20. Click on Accept Changes.
    ![](assets/os/oel86/capture_24-09-22_20-29-16.png)
21. Click on KDUMP.
    ![](assets/os/oel86/capture_24-09-22_20-30-01.png)
22. Disable the checkbox and click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-29-35.png)
23. Click on Network & Host Name
    ![](assets/os/oel86/capture_24-09-22_20-30-13.png)
24. Enable the interface, change the host name and click on Apply. Click on Done.
    name.domain = oracle.itq.edu.mx
    ![](assets/os/oel86/capture_24-09-22_20-30-51.png)
25. Click on Security Policy.
    ![](assets/os/oel86/capture_24-09-22_20-31-19.png)
26. Turn off the switch and click on Done.
    ![](assets/os/oel86/capture_24-09-22_20-31-31.png)
27. Click on Root Password.
    ![](assets/os/oel86/capture_24-09-22_20-31-47.png)
28.  Set `oracle` as the password. Click twice on Done.
    ![](assets/os/oel86/capture_24-09-22_20-32-01.png)
29. Click on Begin Installation.
    ![](assets/os/oel86/capture_24-09-22_20-33-24.png)
30. The installation begins.
    ![](assets/os/oel86/capture_24-09-22_20-33-37.png)
31. Once finished, click on Reboot System.
    ![](assets/os/oel86/capture_24-09-22_20-58-58.png)
32. Click on License Information.
    ![](assets/os/oel86/capture_24-09-22_21-00-01.png)
33. Accept the agreement and click on Done.
    ![](assets/os/oel86/capture_24-09-22_21-00-13.png)
34. Click on Finish Configuration.
    ![](assets/os/oel86/capture_24-09-22_21-00-28.png)
35. Click on Next.
    ![](assets/os/oel86/capture_24-09-22_21-00-59.png)
36. Input your Full Name and `oracle` as the Username. Click on Next.
    ![](assets/os/oel86/capture_24-09-22_21-02-07.png)
37. Click on Start Using Oracle Linux Server.
    ![](assets/os/oel86/capture_24-09-22_21-03-03.png)
38. Close the Getting Started window.
    ![](assets/os/oel86/capture_24-09-22_21-03-24.png)

### Installing Guest Additions.

1. Open a terminal.
![](assets/os/oel86/capture_24-09-22_21-03-38.png)
2. Update all packages with the following command.
   ```
   sudo yum update
   ```
    ![](assets/os/oel86/capture_24-09-22_21-04-00.png)
3. Input `oracle` as root password.
    ![](assets/os/oel86/capture_24-09-22_21-04-23.png)
4. Input `Y` to confirm the installation.
    ![](assets/os/oel86/capture_24-09-22_21-06-13.png)
5. Again `Y` to confirm.
    ![](assets/os/oel86/capture_24-09-22_21-08-13.png)
6. Installation completed.
    ![](assets/os/oel86/capture_24-09-22_21-23-28.png)
7. Issute the command `uname -r` to check the kernel version.  
   Issue the `yum list kernel*` command and search your kernel version in the output.
    ![](assets/os/oel86/capture_24-09-22_21-25-50.png)
8. In my case I will use the version `kernel-uek-devel.x86_64`
    ![](assets/os/oel86/capture_24-09-22_21-26-11.png)
9. Issue the command `sudo yum install kernel-uek-devel.x86_64`
    ![](assets/os/oel86/capture_24-09-22_21-27-27.png)
10. Input the password for root.
    ![](assets/os/oel86/capture_24-09-22_21-27-40.png)
11. Input `Y` to confirm the installation.
    ![](assets/os/oel86/capture_24-09-22_21-27-58.png)
12. Installation completed.
    ![](assets/os/oel86/capture_24-09-22_21-29-14.png)
13.  Issue the command `sudo reboot` to reboot the system.
    ![](assets/os/oel86/capture_24-09-22_21-39-41.png)
14.  Click on VirtualBox Devices and Insert Guest Additions image.
    ![](assets/os/oel86/capture_24-09-22_21-35-10.png)
15. Click on Run.
    ![](assets/os/oel86/capture_24-09-22_21-35-41.png)
16. Press enter to close the window.
    ![](assets/os/oel86/capture_24-09-22_21-39-27.png)
17. Reboot the system.  
18. Now the Virtual Machine should be as width as the host device screen.
    ![](assets/os/oel86/capture_24-09-22_21-40-58.png)
19. Open the Settings application.
    ![](assets/os/oel86/capture_24-09-22_21-41-10.png)
20. In Power > Power Saving, set Blank Screen to Never.
    ![](assets/os/oel86/capture_24-09-22_21-41-32.png)
21. Power off the system.  
22. Right click on the instance and Clone.
    ![](assets/os/oel86/capture_24-09-22_21-43-42.png)
23. Append "GuessAdd" to the name and checkbox in Keep Disk Names.
    ![](assets/os/oel86/capture_24-09-22_21-44-14.png)
24. Click on Full Clone checkbox and Clone.
    ![](assets/os/oel86/capture_24-09-22_21-44-29.png)

## Prepare the OS for the Installation [Prerequisites]

1. Start the original instance and open a terminal.
2. Check the Internet connectivity using:
    ```bash
    ping www.google.com -c 4
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-36-51.png)

3. Check cache to download the metadata from online repo:
    ```bash
    dnf makecache
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-37-22.png)

4. Install prerequisites, enter the password:
    ```bash
    sudo dnf install oracle-database-preinstall-19c -y 
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-37-55.png)
    ![](assets/os/prerequisites/capture_25-09-22_00-38-46.png)

5. This step is optional if you want to update the system:
    ```bash 
    dnf check-update
    dnf update 
    dnf clean all
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-39-14.png)
    ![](assets/os/prerequisites/capture_25-09-22_00-39-25.png)
    ![](assets/os/prerequisites/capture_25-09-22_00-39-40.png)

6. Login with root and input the password.
    ```bash
    sudo -i
    ```

7. Append the recently created groups by the installer to the `oracle` user
    ```bash
    usermod -a -G oinstall,racdba,dba,oper,backupdba,dgdba,kmdba oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-41-18.png)

8. Verify the groups of user `oracle`
    ```bash
    id oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-43-38.png)

9.  Create OS groups for asm administration and operation:
    ```bash
    groupadd -g 54327 asmdba
    groupadd -g 54328 asmoper
    groupadd -g 54329 asmadmin
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-44-01.png)

10. Add as secondary group to oracle user:
    ```bash
    usermod -a -G asmadmin,asmdba oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-44-25.png)

11. Create Grid user:
    ```bash
    useradd -u 54331 -g oinstall -G dba,asmdba,asmadmin,asmoper,racdba grid
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-45-08.png)

12. Change the password for Oracle and Grid user to `oracle`:
    ```bash
    passwd oracle
    passwd grid
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-45-35.png)
    ![](assets/os/prerequisites/capture_25-09-22_00-45-57.png)

13. Create the Directories for the Oracle Grid installation 
    ```bash
    mkdir -p /u01/19c/oracle_base
    mkdir -p /u01/19c/oracle_base/oracle/db_home
    chown -R oracle:oinstall /u01
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-46-40.png)

14. Create the Directories for the Oracle Database installation 
    ```bash
    mkdir -p /u01/19c/grid_base
    mkdir -p /u01/19c/grid_home
    chown -R grid:oinstall /u01/19c/grid_base /u01/19c/grid_home
    chmod -R 775 /u01
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-47-01.png)

15. Switch to the `grid` user and edit the Grid `.bash_profile`, before edit the file I will take backup for it first
    ```bash
    su - grid
    cd /home/grid
    cp .bash_profile .bash_profile.bkp
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-47-31.png)

16. Copy and paste this to grid home directory 
    ```bash
    cat > /home/grid/.grid19c_env <<EOF
    # User specific environment and startup programs
    ORACLE_SID=+ASM; export ORACLE_SID
    ORACLE_BASE=/u01/19c/grid_base; export ORACLE_BASE
    ORACLE_HOME=/u01/19c/grid_home; export ORACLE_HOME
    ORACLE_TERM=xterm; export ORACLE_TERM
    JAVA_HOME=/usr/bin/java; export JAVA_HOME
    TNS_ADMIN=\$ORACLE_HOME/network/admin; export TNS_ADMIN
    PATH=.:\${JAVA_HOME}/bin:\${PATH}:\$HOME/bin:\$ORACLE_HOME/bin
    PATH=\${PATH}:/usr/bin:/bin:/usr/local/bin
    export PATH
    umask 022
    EOF
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-47-59.png)

17. Apply the profile for the current session and check the environment variables:
    ```bash
    echo "source ~/.grid19c_env" >> ~/.bash_profile
    exit
    su - grid
    env | grep -i oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-51-37.png)

18. Switch to `oracle` user and backup the `.bash_profile` :
    ```bash
    su - oracle 
    cp .bash_profile .bash_profile.bkp
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-52-32.png)
    ![](assets/os/prerequisites/capture_25-09-22_00-52-51.png)

19. Create new bash profile file copy the below script to your terminal and press enter:
    ```bash
    cat > /home/oracle/.db19c_env <<EOF
    # specific environment and startup programs
    ORACLE_HOSTNAME=\$HOSTNAME; export ORACLE_HOSTNAME
    ORACLE_SID=prod; export ORACLE_SID
    ORACLE_UNQNAME=prod; export ORACLE_UNQNAME
    ORACLE_BASE=/u01/19c/oracle_base; export ORACLE_BASE
    ORACLE_HOME=\$ORACLE_BASE/oracle/db_home; export ORACLE_HOME
    ORACLE_TERM=xterm; export ORACLE_TERM
    JAVA_HOME=/usr/bin/java; export JAVA_HOME
    TNS_ADMIN=\$ORACLE_HOME/network/admin; export TNS_ADMIN
    PATH=.:\${JAVA_HOME}/bin:\${PATH}:\$HOME/bin:\$ORACLE_HOME/bin
    PATH=\${PATH}:/usr/bin:/bin:/usr/local/bin
    NLS_DATE_FORMAT="DD-MON-YYYY HH24:MI:SS"; export NLS_DATE_FORMAT
    TNS_ADMIN=\$ORACLE_HOME/network/admin; export TNS_ADMIN
    PATH=.:\${JAVA_HOME}/bin:\${PATH}:\$HOME/bin:\$ORACLE_HOME/bin
    PATH=\${PATH}:/usr/bin:/bin:/usr/local/bin
    TEMP=/tmp ;export TMP
    TMPDIR=\$tmp ; export TMPDIR
    export PATH
    umask 022
    EOF
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-53-21.png)

20. Test
    ```bash
    . .db19c_env
    env | grep -i oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-57-24.png)

21. Apply the profile 
    ```bash
    echo "source ~/.db19c_env" >> ~/.bash_profile
    exit
    su - oracle
    env | grep -i oracle
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-57-54.png)

22. Login with root and input the password.
    ```bash
    sudo -i
    ```

23. Check the NTP service 
    ```bash
    systemctl status chronyd
    ```

24. If the service is inactive, issue the following commands:
    ```bash
    systemctl start chronyd
    systemctl enable chronyd
    ```
    ![](assets/os/prerequisites/capture_25-09-22_00-58-46.png)

25. Set secure linux to permissive 

    ```bash
    # change SELINUX=enforcing to SELINUX=permissive
    sed -i s/SELINUX=enforcing/SELINUX=permissive/g /etc/selinux/config
    cat /etc/selinux/config

    # create limitation in security forlder for grid user 
    cp /etc/security/limits.d/oracle-database-preinstall-19c.conf /etc/security/limits.d/grid-database-preinstall-19c.conf

    # rename oracle with grid in this file grid-database-preinstall-19c.conf
    # use vim 
    vim /etc/security/limits.d/grid-database-preinstall-19c.conf
    ```
    :%s/oracle/grid/g  
    :x
    ![](assets/os/prerequisites/capture_25-09-22_01-00-34.png)
    ![](assets/os/prerequisites/capture_25-09-22_01-01-05.png)
    ![](assets/os/prerequisites/capture_25-09-22_01-01-26.png)
    ![](assets/os/prerequisites/capture_25-09-22_01-01-53.png)
    ![](assets/os/prerequisites/capture_25-09-22_01-02-10.png)

26. Disable Linux firewall [ref-link](https://www.ateam-oracle.com/opening-ports-in-linux-7-firewalls-for-oracle-analytics-cloud-access-to-databases-and-remote-data-connector)
    ```bash
    systemctl stop firewalld
    systemctl disable firewalld
    ```
    ![](assets/os/prerequisites/capture_25-09-22_01-02-45.png)

27. At this point you want to create a Snapshot or a clone of the instance.

## Create and Tag ASM Disks.

1. Download [ASMLib8](https://www.oracle.com/linux/downloads/linux-asmlib-v8-downloads.html)

2. Download [oracleasm-support](https://public-yum.oracle.com/repo/OracleLinux/OL8/addons/x86_64/getPackage/oracleasm-support-2.1.12-1.el8.x86_64.rpm)
   
3. Go to the downloads folder.
    ```bash
    cd Downloads
    ```

4. Install the packages with the following command.
    ```bash
    sudo yum localinstall ./oracleasm-support-2.1.12-1.el8.x86_64.rpm ./oracleasmlib-2.0.17-1.el8.x86_64.rpm
    ```
    **Versions may vary*
    ![](assets/grid/disks/capture_25-09-22_01-11-10.png)
    ![](assets/grid/disks/capture_25-09-22_01-11-21.png)
    ![](assets/grid/disks/capture_25-09-22_01-11-35.png)

5. Reboot the system
6. Login with root and input the password.
    ```bash
    sudo -i
    ```
    ![](assets/grid/disks/capture_25-09-22_01-19-13.png)
7. Start configuring ASM with the following command.
    ```bash
    oracleasm configure -i
    ```
    ![](assets/grid/disks/capture_25-09-22_01-19-46.png)
    
8. Issue the following command:
    ```bash
    oracleasm init
    ```
    ![](assets/grid/disks/capture_25-09-22_01-20-18.png)

9. List the disk to tag:
    ```bash
    cd /dev/
    pwd
    ls | grep sd
    ```
    ![](assets/grid/disks/capture_25-09-22_01-21-00.png)

10. Format the disks from *b* to *i*
    ```bash
    fdisk /dev/sd*i*
    p
    n
    p
    1
    [intro]
    [intro]
    w
    ```
    ![](assets/grid/disks/capture_25-09-22_01-22-23.png)

11. Check the formated disks.
    ```bash
    ls | grep sd
    ```
    ![](assets/grid/disks/capture_25-09-22_01-26-01.png)

12. Tag the formatted disks as ASM Disks
    ```bash
    oracleasm createdisk ASMDISKn /dev/sd*i*1
    ```
    ![](assets/grid/disks/capture_25-09-22_01-27-52.png)

13. Check the created ASM disks.
    ```bash
    oracleasm listdisks
    ```
    ![](assets/grid/disks/capture_25-09-22_01-28-15.png)

## Installing Oracle Grid Infrastructure 19c Software 

1. Switch to the `grid` user and input the password.
    ```bash
    su - grid
    ```
    ![](assets/grid/software/capture_25-09-22_01-29-06.png)

2. Change to home directory.
   ```bash
   cd $ORACLE_HOME
   ```
   ![](assets/grid/software/capture_25-09-22_01-29-29.png)

3. Download the [Oracle Grid 19c Software](https://www.oracle.com/database/technologies/oracle19c-linux-downloads.html#license-lightbox) in the home directory (current directory).
    ![](assets/grid/software/capture_25-09-22_01-31-15.png)
    ![](assets/grid/software/capture_25-09-22_01-31-25.png)
    ![](assets/grid/software/capture_25-09-22_01-32-01.png)
    ![](assets/grid/software/capture_25-09-22_01-32-11.png)
    ![](assets/grid/software/capture_25-09-22_01-34-54.png)
    ![](assets/grid/software/capture_25-09-22_01-36-22.png)
    ![](assets/grid/software/capture_25-09-22_01-36-38.png)
    ![](assets/grid/software/capture_25-09-22_01-37-03.png)
    ![](assets/grid/software/capture_25-09-22_01-37-28.png)
    ![](assets/grid/software/capture_25-09-22_01-37-41.png)
    ![](assets/grid/software/capture_25-09-22_01-38-00.png)
    ![](assets/grid/software/capture_25-09-22_01-38-12.png)

4. Unzip the Oracle Grid 19c Software
    ```bash
    unzip LINUX.X64_193000_grid_home.zip
    ```
    ![](assets/grid/software/capture_25-09-22_12-15-17.png)

5. Issue the command:
    ```bash
    export CV_ASSUME_DISTID=OEL8.6
    ```
    ![](assets/grid/software/capture_25-09-22_01-40-21.png)

6. Run the installer.
    ```bash
    ./gridSetup.sh
    ```
    ![](assets/grid/software/capture_25-09-22_01-40-47.png)

7. If the output is:
    ```bash
    ERROR: Unable to verify the graphical display setup. This application requires X display. Make sure that xdpyinfo exist under PATH variable.
    ```
    Close the terminal and repeat steps 1, 2 and 5.

8.  Select Configure Oracle Grid Infrastructure for a Standalone Server (Oracle Restart)
    ![](assets/grid/software/capture_25-09-22_12-20-12.png)

9.  Select 1MB size Allocation Unit.
    ![](assets/grid/software/capture_25-09-22_12-21-17.png)
    
10. Select Change Disk Discovery Path, input `/dev/oracleasm/disks` and click on OK.
    ![](assets/grid/software/capture_25-09-22_12-21-39.png)
    ![](assets/grid/software/capture_25-09-22_12-22-13.png)

11. Select the first 4 disks and click Next.
    ![](assets/grid/software/capture_25-09-22_12-22-47.png)

12. Select the Use same password for these accouts checkbox. Input `oracle` as the password. Click on Next. Ignore the warning.
    ![](assets/grid/software/capture_25-09-22_12-23-29.png)
    ![](assets/grid/software/capture_25-09-22_12-23-57.png)
    
13. Click on Next.
    ![](assets/grid/software/capture_25-09-22_12-24-19.png)

14. Click on Next.
    ![](assets/grid/software/capture_25-09-22_12-24-35.png)

15. Click on Next.
    ![](assets/grid/software/capture_25-09-22_12-24-57.png)

16. Click on Next.
    ![](assets/grid/software/capture_25-09-22_12-25-21.png)

17. Click on Next.
    ![](assets/grid/software/capture_25-09-22_12-25-58.png)

18. Select the missing requisite and click on Fix and Check Again.
    ![](assets/grid/software/capture_25-09-22_01-48-26.png)

19. Copy the path to the script to execute.
    ![](assets/grid/software/capture_25-09-22_01-48-50.png)

20. Open a terminal and login with root.
    ```bash
    sudo -i
    ```
    ![](assets/grid/software/capture_25-09-22_01-49-06.png)

21. Paste the path to the script and execute it.
    ![](assets/grid/software/capture_25-09-22_01-49-39.png)

22. Get back to the Grid Installation and Click on OK.
    ![](assets/grid/software/capture_25-09-22_01-49-49.png)

23. Click on Install.
    ![](assets/grid/software/capture_25-09-22_01-49-58.png)

24. If prompt to execute scripts, copy the paths to the scripts and run them in the root terminal.
    ![](assets/grid/software/capture_25-09-22_01-51-30.png)
    ![](assets/grid/software/capture_25-09-22_01-53-45.png)

25. Get back to the installation and click on OK.
    ![](assets/grid/software/capture_25-09-22_01-54-33.png)

26. Installation is completed.
    ![](assets/grid/software/capture_25-09-22_01-56-03.png)

### Creating FRA Disk Group.

1. Open a terminal and switch to `grid` user and input the password.
   ```bash
   su - grid
   ```
   ![](assets/grid/software/capture_25-09-22_12-56-50.png)
   
2. Enviroment.
   ```bash
   . oraenv
   +ASM
   ```
   ![](assets/grid/software/capture_25-09-22_13-01-43.png)

3. Open ASM Configuration Assistant.
   ```bash
   asmca
   ```
   ![](assets/grid/software/capture_25-09-22_13-02-01.png)

4. Expand Disk Groups. Click on Create.
    ![](assets/grid/software/capture_25-09-22_13-02-21.png)
   
5. Use `FRA` as the Disk Group Name. Select External Redundancy. Select 1MB Allocation Unit Size. Select the 4 disks available. Finally Click on OK.
   ![](assets/grid/software/capture_25-09-22_13-03-13.png)
   
6. There will be 2 Disk Groups.
   ![](assets/grid/software/capture_25-09-22_13-04-15.png)

7. You might want to create a Snapshot or a clone of the instance before installing Oracle Database.

## Installing Oracle DB 19c software Only

1. Switch to the `oracle` user and input the password.
    ```bash
    su - oracle
    ```
    ![](assets/db/software/capture_25-09-22_13-44-34.png)

2. Change to home directory.
   ```bash
   cd $ORACLE_HOME
   ```
   ![](assets/db/software/capture_25-09-22_13-45-02.png)

3. Download [Oracle Database 19c](https://www.oracle.com/database/technologies/oracle19c-linux-downloads.html#license-lightbox) and open a terminal in the download folder.
    ![](assets/db/software/capture_25-09-22_13-46-25.png)
    ![](assets/db/software/capture_25-09-22_13-46-36.png)
    ![](assets/db/software/capture_25-09-22_13-47-04.png)
    ![](assets/db/software/capture_25-09-22_13-47-17.png)
    ![](assets/db/software/capture_25-09-22_13-47-30.png)
    ![](assets/db/software/capture_25-09-22_13-51-44.png)
    ![](assets/db/software/capture_25-09-22_13-52-00.png)

4. Unzip the oracle database 19c zipped files in the home dir location :
    ```bash
    unzip LINUX.X64_193000_db_home.zip -d $ORACLE_HOME
    ```
    ![](assets/db/software/capture_25-09-22_13-52-27.png)

5. Run the following command
    ```bash
    export CV_ASSUME_DISTID=OEL8.6
    ```
    ![](assets/db/software/capture_25-09-22_13-54-19.png)

6. Run the installer.
   ```bash
   ./runInstaller
   ```
   ![](assets/db/software/capture_25-09-22_13-54-37.png)

7. Select the Set up software only checkbox. Click on Next.
   ![](assets/db/software/capture_25-09-22_13-55-12.png)

8. Select Single instance database installation. Click on Next.
    ![](assets/db/software/capture_25-09-22_13-55-35.png)

9.  Select Enterprise Edition. Click on Next.
    ![](assets/db/software/capture_25-09-22_13-56-01.png)

10. Click on Next.
    ![](assets/db/software/capture_25-09-22_13-56-22.png)

11. Click on Next.
    ![](assets/db/software/capture_25-09-22_13-56-40.png)

12. Click on Next.
    ![](assets/db/software/capture_25-09-22_13-57-01.png)

13. If any warnings, click on it, and click on Fix and Check Again.
    ![](assets/db/software/capture_25-09-22_13-57-28.png)

14. Copy the path to the script and run it as root.
    ![](assets/db/software/capture_25-09-22_13-57-48.png)
    ![](assets/db/software/capture_25-09-22_13-58-23.png)
    ![](assets/db/software/capture_25-09-22_13-58-42.png)

15. Get back to the installation and click on OK.
    ![](assets/db/software/capture_25-09-22_13-59-01.png)

16. Click on Install.
    ![](assets/db/software/capture_25-09-22_13-59-13.png)

17. When prompt to run root scripts. Copy the path to the script and run it as root.
    ![](assets/db/software/capture_25-09-22_14-01-07.png)
    ![](assets/db/software/capture_25-09-22_14-02-01.png)

18. Get back to the installation and click on OK.
    ![](assets/db/software/capture_25-09-22_14-02-29.png)

19. Installation is completed.
    ![](assets/db/software/capture_25-09-22_14-02-37.png)

## Create database using dbca.


1. Open the DataBase Configuration Assistant.
   ```bash
   dbca
   ```
    ![](assets/db/instance/capture_25-09-22_14-26-58.png)

2. Select Create a Database and click on Next.
    ![](assets/db/instance/capture_25-09-22_14-27-29.png)

3. Select Advanced Configuration and click on Next.
    ![](assets/db/instance/capture_25-09-22_14-27-53.png)

4. Click on Next.
   ![](assets/db/instance/capture_25-09-22_14-28-13.png)

5. Click on Next.
   ![](assets/db/instance/capture_25-09-22_14-28-28.png)

6. Select Use following for the database storage attributes. Browse for Database file location and select `DATA` Disk Group. Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-29-11.png)
    ![](assets/db/instance/capture_25-09-22_14-29-25.png)
    ![](assets/db/instance/capture_25-09-22_14-29-39.png)

7. Select Specify Fast Recovery Area, browse for Fast Recovery Area and select FRA Disk Group. Set a 10809 MB Fast Recovery Area size. Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-30-39.png)
    ![](assets/db/instance/capture_25-09-22_14-30-53.png)
    ![](assets/db/instance/capture_25-09-22_14-31-06.png)
   
8. Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-31-42.png)

9.  Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-32-25.png)

10. Set a SGA Size of 529 MB and a PGA Size of 100 MB.
    ![](assets/db/instance/capture_25-09-22_14-33-02.png)

11. Go to Sample Schemas adn enable Add sample schemas to the database checkbox. Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-33-32.png)

12. Disable the checkboxes and click on Next.
    ![](assets/db/instance/capture_25-09-22_14-34-15.png)

13. Select Use the same adminsitrative password for all accounts. Set `oracle` as the password. Click on Next.
    ![](assets/db/instance/capture_25-09-22_14-34-35.png)

14. Click on Yes to continue.
    ![](assets/db/instance/capture_25-09-22_14-35-01.png)

15. Select Create database and click on Next.
    ![](assets/db/instance/capture_25-09-22_14-35-17.png)

16. Click on Finish
    ![](assets/db/instance/capture_25-09-22_14-35-40.png)

17. The Database is created.
    ![](assets/db/instance/capture_25-09-22_15-07-52.png)

## Verifying the installation.

1. Eviroment.
   ```bash
   . oraenv
   orcl
   ```
   ![](assets/db/instance/capture_25-09-22_15-14-36.png)

2. Login in SQL*Plus.
   ```bash
   sqlplus
   sys as sysdba
   oracle
   ```
    ![](assets/db/instance/capture_25-09-22_15-15-08.png)

3. Test the following queries.
   ```sql
   select instance_name from v$instance;
   select name from v$controlfile;
   ```
   ![](assets/db/instance/capture_25-09-22_15-15-51.png)