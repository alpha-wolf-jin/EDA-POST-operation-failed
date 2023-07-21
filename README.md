# EDA-POST-operation-failed

**When active the rulebook in AAP2.4 EDA console, encounted the below error**
```
Pulling image quay.io/albert2013/de-kafka-rhel9-03:latest
Activation eda_rulebase_02 failed: http://%2Frun%2Fuser%2F993%2Fpodman%2Fpodman.sock/v4.5.0/libpod/images/pull (POST operation failed)
```

**Solution**

On EDA server
```
# su - eda
[eda@eda ~]$ podman images
ERRO[0000] invalid internal status, try resetting the pause process with "podman system migrate": could not find any running process: no such process 

[eda@eda ~]$ podman system migrate
stopped 1dd7b019bdf93c511e6e99256ff095608710c0f516d5458b6626f2e6522975a3


[eda@eda ~]$ podman ps -a
CONTAINER ID  IMAGE                                        COMMAND               CREATED       STATUS      PORTS                   NAMES
1dd7b019bdf9  quay.io/albert2013/de-kafka-rhel9-02:latest  ansible-rulebook ...  23 hours ago


[eda@eda ~]$ podman rm 1dd7b019bdf9 --force
ERRO[0000] Free container lock: no such file or directory 
1dd7b019bdf9

[eda@eda podman]$ rm /run/user/993/podman/podman.sock 


[eda@eda podman]$ exit


# reboot

```
**When active the rulebook in AAP2.4 EDA console, encounted the below error**
```
Attempting to login to registry: quay.io

Activation rulebase-01 failed: http://%2Frun%2Fuser%2F993%2Fpodman%2Fpodman.sock/v1.40/auth (POST operation failed)
```
This solution does not work.
https://access.redhat.com/solutions/6771781

**Solution**
On EDA server
```
[root@eda ~]# su - eda
Last login: Fri Jul 21 11:10:20 +08 2023 on pts/0

[eda@eda ~]$ podman images
Error: cannot re-exec process to join the existing user namespace

[eda@eda ~]$ podman ps
Error: cannot re-exec process to join the existing user namespace

[eda@eda ~]$ exit
logout

[root@eda ~]# find /tmp -name "*pause*"
/tmp/ansible.gm3vytl7/libpod/tmp/pause.pid

[root@eda ~]# rm -f /tmp/ansible.gm3vytl7/libpod/tmp/pause.pid

[root@eda ~]# su - eda

[eda@eda ~]$ podman ps
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES

# reboot
```





