# Lab: Terraform State

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Which location is the terraform state file stored by default?</summary>

    > Inside the configuration directory.

    This keeps it associated with the configuration.

    </details>

1.  <details>
    <summary>Which option should we use to disable state?</summary>

    > We cannot disable state!

    State is the thing which makes terraform possible. Without it we have nothing.

    </details>

1.  <details>
    <summary>Which format is the state file stored in by default?</summary>

    > JSON

    </details>

1.  <details>
    <summary>Which of the following commands does NOT refresh the state?</summary>

    > terraform init

    * `plan` refreshes the state with the state of currently deployed resources, if you have previously applied the configuration.
    * `apply` updates the state with any changes made.

    </details>

1.  <details>
    <summary>What is the name of the state file that is created by default?</summary>

    > `terraform.tfstate`

    </details>

1.  <details>
    <summary>Navigate to the configuration directory /root/terraform-projects/project-flash we have created a few configuration files here.</summary>

    The directory has been initialized and the provider plugins downloaded inside the .terraform directory. However, there is no terraform.tfstate file created. Why is that?

    > `terraform apply` was not run

    Until some resources have actually been deployed, there is no state.

    </details>

1.  <details>
    <summary>Run the terraform show command and identify the id created for the resource called speed_force.</summary>

    ```bash
    cd /root/terraform-projects/project-flash/
    terraform show
    ```

    > No details printed. There is no state.

    We haven't applied anything yet.

    </details>

1.  <details>
    <summary>Now, run terraform apply in this directory.</summary>

    ```bash
    terraform apply
    ```

    </details>

1.  <details>
    <summary>Now, check terraform show again. What is the value of id for the resource called speed_force?</summary>

    ```bash
    terraform show
    ```

    Locate the `id` attribute of the `speed_force` resource

    </details>

1.  Information only

1.  <details>
    <summary>Inspect the terraform.tfstate file or run terraform show command.
    You will notice that all the attribute details for all the resources created by this configuration is now printed on the screen!<br/>Among them is an EC2 Instance which is created by the resource called dev-server. See if you can find out the private_ip for the instance that was created.</summary>

    ```bash
    cd /root/terraform-projects/project-flash/
    terraform show
    ```

    Locate the `private_ip` attribute of the `aws_instance.dev-server` resource

    </details>

1.  Information only

