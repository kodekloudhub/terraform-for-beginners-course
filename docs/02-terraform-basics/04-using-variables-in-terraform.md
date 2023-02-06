# Lab: Using Variables in Terraform

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>How can we use environment variables to pass input variables in terraform scripts?</summary>

    Terraform looks for exported environment variables with names beginning `TF_VAR_`

    > `TF_VAR_<variable_name>`

    </details>

1.  <details>
    <summary>Which method has the highest priority in Variable Definition Precedence?</summary>

    Refer to the [documentation](https://developer.hashicorp.com/terraform/language/values/variables#variable-definition-precedence)

    > Command line flags `-var` or `-var-file`

    </details>

1.  <details>
    <summary>Which one of the following commands is a valid way to make use of a custom variable file with the terraform apply command?</summary>

    By convention, variables files have the extension `.tfvars` or `.tfvars.json`

    > `terraform apply -var-file variables.tfvars`

    </details>

1.  Information only

1.  <details>
    <summary>What will happen if we run terraform plan command right now?</summary>

    ```
    terraform plan
    ```

    Inspect the output.

    </details>

1.  Information only

1.  <details>
    <summary>Declare the variable called filename with type string in the file variables.tf.<br/>Don't have to specify a default value.</summary>

    1. Add file `variables.tf`
    1. Insert variable definition

        <details>
        <summary>Reveal</summary>

        ```
        variable "filename" {
            type = string
        }
        ```
        </details>
    </details>

1.  <details>
    <summary>If we run terraform apply with a -var command line flag as shown below, which value would be considered by terraform?</summary>

    Don't run this now:

    `terraform apply -var filename=/root/tennis.txt`


    We are explicitly setting the value of the input variable created in Q7 to `/root/tennis.txt`

    </details>

1.  Information only


