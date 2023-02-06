# Lab: Multiple Providers

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Can we use multiple providers in the same configuration directory?</summary>

    > `Yes`

    </details>

1.  <details>
    <summary>We have a new configuration directory located at the path /root/terraform-projects/multi-provider. Inspect this directory and find out the number of providers initialized.</summary>

    As with the previous lab, we need to inspect the given directory to see if it contains a `.terraform` directory, and if so count the providers within.

    > There are none, becuase `terraform init` has not been run.

    </details>

1.  <details>
    <summary>Now, run the terraform init command and inspect the .terraform/plugins directory. Count the number of plugins downloaded.</summary>

    ```
    cd /root/terraform-projects/multi-provider
    terraform init
    ```

    We can either inspect the plugins directory as stated above, or examine the output of the above command.

    </details>

1.  <details>
    <summary>How many plugins are available now in this configuration directory?</summary>

    You've already counted them in the previous question :smile:

    </details>

1.  <details>
    <summary>Now, Navigate to the directory /root/terraform-projects/MPL. Create a new configuration file called pet-name.tf.</summary>

    This file should make use of the `local_file` and `random_pet` resource type with the below requirements:

    `local_file` resource details:

    * Resource name = "`my-pet`"
    * File name = "`/root/pet-name`"
    * Content = "`My pet is called finnegan!!`"
    <br/><br/>

    `random_pet` resource details:

    * Resource name = "`my-pet`"
    * Length = "`1`"
    * Prefix = "`Mr`"
    * Separator = "`.`"

    1. Navigate to `terraform-projects/MPL` in the Explorer pane.
    1. Add file `pet-name.tf`
    1. Add the `local_file` resource

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "my-pet" {
            filename = "/root/pet-name"
            content = "My pet is called finnegan!!"
        }
        ```

        </details>

    1. Add the `random_pet` resource

        <details>
        <summary>Reveal</summary>

        ```
        resource "random_pet" "other-pet" {
            length = "1"
            prefix = "Mr"
            separator = "."
        }
        ```

        </details>

    1. Open the termninal window and run

        ```bash
        cd /root/terraform-projects/MPL
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>Now change into the directory /root/terraform-projects/provider and inspect the file cloud-provider.tf.<br/>What is the instance_type configured with the resource type called aws_instance?</summary>

    1. Navigate to `terraform-projects/provider` in the Explorer pane and click on the indicated file to open it.
    1. Get the value of the argument `instance_type` on the `aws_instance` resource

    > `t2.large`

    </details>

1.  <details>
    <summary>What is the name of the resource configured with the resource type kubernetes_namespace in kube.tf file within the same directory?</summary>

    1. Click on the indicated file to open it.
    1. Know that the name of a resource is the second argument to the `resource` statement

    > `dev`


    </details>

1.  <details>
    <summary>Let's get some more practice! Now navigate to the directory path /root/terraform-projects/provider-a. Create a configuration file called code.tf.</summary>

    Using the local_file resource type, write the resource block with the below requirements into the file:

    * Resource name = `iac_code`
    * File name = `/opt/practice`
    * Content = `Setting up infrastructure as code`

    When ready, _only_ run the terraform init command, we will run the terraform apply command later on.

    1. Navigate to `terraform-projects/proider-a` in the Explorer pane.
    1. Add file `code.tf`
    1. Add the `local_file` resource

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "iac_code" {
            filename = "/opt/practice"
            content = "Setting up infrastructure as code"
        }
        ```

        </details>

    1. Open the termninal window and run

        ```bash
        cd /root/terraform-projects/provider-a
        terraform init
        ```

    </details>

1.  <details>
    <summary>We have made some changes to the configuration file. Are you able to run terraform apply command?</summary>

    1. In the terminal, run

        ```
        terraform apply
        ```

        Did it succeed?

    </details>

1.  <details>
    <summary>Fix it</summary>

    This is because whenever we add a resource for a provider that has not been used so far in the configuration directory, we have to initialize the directory by running terraform init command

    Run the following

    ```
    terraform init
    terraform apply
    ```

    </details>

