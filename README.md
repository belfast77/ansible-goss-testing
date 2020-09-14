### Verification and Validation Testing with GOSS and Ansible

"One of the most important tests to run when deploying an IT solution is infrastructure verification and validation (V&V). In other words, making sure the system is configured the way it is supposed to be. Typically, V&V is one of the final phases of the test cycle and it is one of the most important. The goal of “V&V” is to make sure that it detects any inconsistencies between the system units that are integrated together or inconsistencies between deployment tiers such as Dev, Test, QA, Pre-Prod and Production. Obviously, this can be a major project, especially in organisations with lots of system components. Even for a small implementation it takes a lot of effort to make sure everything is working correctly. The risk of failure in this work is extremely high. To prevent this, requires a major effort leading up to go-live. The implementation team needs to verify all the details to make sure things are working properly. This adds up to a lot of effort to head off problems before the system goes live." *

The ability to automate the bulk of testing brings huge benefits in terms of time to complete and consistency of results.

GOSS stands for “GO Server Spec” and is an opensource tool and source code can be found at [https://github.com/aelsabbahy/goss](https://github.com/aelsabbahy/goss) .

Many people develop their infrastructure as code, initially on Laptops with Vagrant and Virtulabox etc, then promote to an official dev environment which maybe in AWS, then a test environment which maybe on prem or different cloud platform or spec, then UIT, Pre-Pre-Prod and Prod, all may have variations or different teams managing them.

Differences can cause issues and therefore full V&V testing is really invaluable.

GOSS is exceptionally easy to deploy an use, this presentation shows how you can use Ansible and GOSS together to verify and validate you deployments


### Goss Playbooks Usage

SSH to an Ansible Control server (server where you run ansible from and has SSH connection to all VMs)

```
git clone git@gitlab.example.com:ansible/projects/ansible-goss-testing.git

cd goss-testing

export ANSIBLE_HOST_KEY_CHECKING=False

```


Basic test creation for OS validation, pre application deployment
```
ansible-playbook -u ansible -kK -b -i hosts -e@extra.vars  goss_auto_add.yml
```



Automated test file creation playbook.

```
ansible-playbook -u ansible -kK -b -i hosts -e@extra.vars  goss_auto_create_all_checks.yaml

```

All test files will be created in ./test-files/

Review these file and modify/customize hostnames and other server specfic data if required for the target servers.

```

ansible-playbook -u ansible -kK -b -i hosts -e@extra.vars -e "goss_test_file=jenkins_goss_tests_jenkins.cowlan.co.uk.yaml" goss_validate.yml


Breakdown of the above command.

>  :: Runs Ansible playbooks, executing the defined tasks on the targeted hosts.
>
>     -u, --user              REMOTE_USER
> 
>     -k, --ask-pass          ask for connection password
>     
>     -b, --become            run operations with become (does not imply password prompting)
> 
>     -i INVENTORY            specify inventory host path or comma separated host list. 
> 
>     goss_validate.yml       The Playbook that run the goss test files.
>     
>     --extra-vars "goss_test_file=jenkins_goss_tests.yaml"  (this points to the actual test file for the spefic server type or server)
>     
>     --limit jenkins      Limit this task to jenkins group in the host file hosts


```


### Validation

```
ansible-playbook -u ansible -kK -b -i hosts -e@extra.vars -e "goss_test_file=jenkins_goss_tests_jenkins.cowlan.co.uk.yaml" goss_validate.yml

```




