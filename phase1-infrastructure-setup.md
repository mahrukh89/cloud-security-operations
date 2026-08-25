> **CY464 --- Cloud Security**
>
> Group Participant: Mahrukh, Shaina, Laiba
>
> Phase 1 --- Infrastructure Setup
>
> Operating System: Ubuntu Linux

**System Environment:**

This phase was executed using an Ubuntu Linux environment configured
with a standard NAT/Bridged network interface to ensure stable
connectivity and repository access during initialization.

**Screenshots:**

1.  Update the system

![](../screenshots/phase1/01-update-system.png){width="6.5in" height="1.3979166666666667in"}

2.  Install docker:

![](../screenshots/phase1/02-install-docker.png){width="6.5in" height="2.8222222222222224in"}

3.  Setup docker

![](../screenshots/phase1/03-setup-docker.png){width="6.5in" height="1.625in"}

4.  Change directory and nano code file

![](../screenshots/phase1/04-cd-and-nano-code-file-a.png){width="6.5in" height="0.4909722222222222in"}

![](../screenshots/phase1/04-cd-and-nano-code-file-b.png){width="6.5in" height="4.270138888888889in"}

5.  Compile the code

![](../screenshots/phase1/05-compile-code-a.png){width="6.5in" height="1.6381944444444445in"}

![](../screenshots/phase1/05-compile-code-b.png){width="6.5in" height="2.6319444444444446in"}

![](../screenshots/phase1/05-compile-code-c.png){width="6.5in" height="1.7902777777777779in"}

6.  Compose and launch

![](../screenshots/phase1/06-compose-and-launch.png){width="6.5in" height="1.4381944444444446in"}

7.  To seamlessly access and test the running web services from the host
    machine\'s Google Chrome window, local DNS mapping was configured.
    The static or DHCP-assigned IP address of the Ubuntu virtual machine
    was retrieved. This IP address was then explicitly added to the
    Windows host operating system\'s environment file located at
    C:\\Windows\\System32\\drivers\\etc\\hosts. By mapping the Ubuntu IP
    address to a custom local hostname, the host machine\'s browser was
    able to successfully resolve, route, and load the active web
    applications directly from the virtualized environment.

8.  Final:

-   Keycloak:

![](../screenshots/phase1/07-keycloak-a.png){width="6.5in" height="3.2in"}

![](../screenshots/phase1/07-keycloak-b.png){width="6.5in" height="3.2666666666666666in"}

-   Nextcloud

![](../screenshots/phase1/08-nextcloud-a.png){width="6.5in" height="3.178472222222222in"}

![](../screenshots/phase1/08-nextcloud-b.png){width="6.5in" height="3.1972222222222224in"}

-   Minio

> ![](../screenshots/phase1/09-minio-a.png){width="6.5in"
> height="3.0520833333333335in"}![](../screenshots/phase1/09-minio-b.png){width="6.5in"
> height="3.1333333333333333in"}

-   Vault:

> ![](../screenshots/phase1/10-vault-a.png){width="6.5in"
> height="3.4569444444444444in"}![](../screenshots/phase1/10-vault-b.png){width="6.5in"
> height="3.5854166666666667in"}
