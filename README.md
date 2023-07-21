# EDA-POST-operation-failed

## Issue 01
**When active the rulebook in AAP2.4 EDA console, encountered the below error**
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

## Issue 02
**When active the rulebook in AAP2.4 EDA console, encountered the below error**
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
## Issue 03

**When the EDA triggers the job template, it should below error in history**
```
2023-07-21 05:00:35,518 - ansible_rulebook.builtin - ERROR - Failed to post to https://aap-eda.example.com/api/v2/job_templates/10/launch/. Status: 400, Body: {"playbook":["Missing a revision to run due to failed project update."]}
```

**Solution**

Found the related project sync failed on the AAP controller. Fix and Re-run.




