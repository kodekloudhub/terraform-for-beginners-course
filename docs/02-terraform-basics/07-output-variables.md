# Lab: Output Variables

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Navigate to indicated directory in the Explorer pane.

1.  <details>
    <summary>Which provider is used by the configuration files in this directory?</summary>

    Know that you can determine provider names from the reource type property of a resource block. The type is of the form `provider_type`.

    Here we can see two distinct resource types:

    * `random_uuid`
    * `random_integer`

    From this, we can determine that there is only one provder in use:

    > `random`

    </details>

1.  <details>
    <summary>Which two resource types are configured in the configuration files?</summary>

    In determining the answer to Q2, we've also determined the answer to this question.

    </details>

1.  Inspect `output.tf`

1.  <details>
    <summary>Run terraform init, plan and apply to create these resources.</summary>

    ```bash
    cd /root/terraform-projects/data
    terraform init
    terraform plan
    terraform apply
    ```

    </details>

1.  <details>
    <summary>What is the value of the output variable called id2 ?</summary>

    Note that we can acutally see all the outputs from the `terraform apply`, however if the screen had been cleared or we were to come back later (in a real environment), we use `terraform output` command.

    ```bash
    terraform output id2
    ```

    The value for `id2` will be printed.

    </details>

1.  <details>
    <summary>What is the value of the output variable called order1 ?</summary>

    ```bash
    terraform output order1
    ```


    </details>

1.  <details>
    <summary>We have a new configuration directory located at the path /root/terraform-projects/output. Inspect the configuration files that are created in this directory.</summary>

    Note that there is a `terraform.tfstate` file in this directory (more on this in later lectures). This means that `terraform apply` has already been run.

    ```bash
    cd /root/terraform-projects/output
    terraform output pet-name
    ```

    </details>

1.  <details>
    <summary>We have just updated the main.tf file in this directory with a new resource block. Add a new output variable...</summary>

    * Output Variable Name: `welcome_message`
    * Value: content of the resource called welcome

    1. Add the output variable block

        <details>
        <summary>Reveal</summary>

        ```
        output "welcome_message" {
            value = local_file.welcome.content
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/output
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

