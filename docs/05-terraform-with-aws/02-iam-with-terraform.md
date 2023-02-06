# Lab: IAM with Terraform

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Information only

1.  <details>
    <summary>Let's start off by creating an IAM User called <b>mary</b> but this time by making use of Terraform. In the configuration directory <b>/root/terraform-projects/IAM<b>, create a file called <b>iam-user.tf</b?</summary>

    * Resource Type: `aws_iam_user`
    * Resource Name: `users`
    * Name: `mary`

    1. Refer to the [documentation]) for `aws_iam_user`. Check the Argument Refrence and note that there's only one required argument. Given the requirements we have, that should be all we need.
    1. Create the resource

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_iam_user" "users" {
            name = "mary"
        }
        ```

        </details>

    1. Init the configuration

        ```bash
        cd /root/terraform-projects/IAM
        terraform init
        ```

    </details>

1.  <details>
    <summary>Run terraform plan within this configuration.</summary>

    ```bash
    terraform plan
    ```

    Note any error.

    </details>

1.  <details>
    <summary>Why did the previous command fail?</summary>

    From the error message we can see that it is

    > Region is not set.

    </details>

1.  Information only

1.  <details>
    <summary>Add a new file called provider.tf containing a provider block for aws.<br/>Inside this block add a single argument called <b>region</b> with the value <b>ca-central-1</b></summary>

    1. Add new file `provider.tf`
    1. Configure the provider block

        <details>
        <summary>Reveal</summary>

        ```
        provider "aws" {
            region = "ca-central-1"
        }
        ```

        </details>


    </details>

1.  <details>
    <summary>Run a terraform plan now. Does it work?</summary>

    ```bash
    terraform plan
    ```

    Note any error.

    </details>

1.  Information only. Note that we have also updated `provider.tf` for you.

1.  <details>
    <summary>Now, run a terraform plan and then a terraform apply</summary>

    ```bash
    terraform plan
    terraform apply
    ```

    </details>

1.  Information only

1.  <details>
    <summary>What is the name of the variable that has been added to the variables.tf file?</summary>

    Inspect `variables.tf`. There's only the one variable.

    </details>

1.  <details>
    <summary>What is the data type used for the variable called <b>project-sapphire-users</b>?</summary>

    Inspect the `type` argument of this variable

    </details>

1.  <details>
    <summary>Now, update the <b>iam-user.tf</b> to make use of the count meta-argument to loop through the <b>project-sapphire-users<b> variable and create all the users in the list.</summary>

    What needs to be done here is almost exactly the same as you did in Q6 in the [count and for_each](../04-working-with-terraform/04-count-and-for-each.md) lab in course section 4.

    1. Update the resource accordingly.

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_iam_user" "users" {
            count = length(var.project-sapphire-users)
            name = var.project-sapphire-users[count.index]
        }
        ```

        </details>

    </details>

