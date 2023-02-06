# Lab: Terraform Commands

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Which command can be used to create a visual representation of our terraform resources?</summary>

    > `terraform graph`

    </details>

1.  <details>
    <summary>We have created a configuration directory /root/terraform-projects/project-shazam. The configuration file inside will be used to create an RSA type private key and then a certificate signing request or a csr using this key.</summary>

    However, there is an error with the configuration. Use the terraform validate command, troubleshoot, and fix the issue.

    You don't have to create the resources yet! You only need to fix the errors reported by terraform validate.

    1. Get details about the error

        ```bash
        cd /root/terraform-projects/project-shazam
        terraform validate
        ```

        It tells us the error with the `private_key` resource

    1. Use the [documentation](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key) to determine what the correct attribute name should be.

    1. Try validating again

        ```
        terraform validate
        ```

        There is another error. We are trying to assign a value to a read-only attribute on the `tls_cert_request` resource ([documentation](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/cert_request#read-only)). Remember that read-only attributes are exactly that - read only. Thus we must _remove_ this attribute from the configuration.

    1.  Validate again

        ```
        terraform validate
        ```

        Success!

    </details>

1.  <details>
    <summary>Great! If you completed the previous question correctly, terraform validate should have passed! Now run terraform plan and generate a configuration plan.</summary>

    ```
    terraform plan
    ```

    > YES

    </details>

1.  <details>
    <summary>Now, try creating the resources with a terraform apply.</summary>

    ```
    terraform apply
    ```

    > NO

    </details>

1.  Information only

1.  <details>
    <summary>The error in the configuration is inside the resource block for the tls_cert_request type resource</summary>

    **NOTE**: The question statement about the cause of the error is currently incorrect, as there has been a change to the `tls` provider since the questions were written. To pass this question, do the following

    * Remove the line `rsa_bits = 4096`

    </details>

1.  <details>
    <summary>Now format the main.tf file into a canonical format.</summary>

    ```bash
    terraform fmt
    ```

    </details>

1.  <details>
    <summary>Now, navigate to the directory /root/terraform-projects/project-a. We have already created the resources specified in this configuration.</summary>

    1. Navigate to the given directory in the Explorer pane
    1. Open the file `terraform.tfstate`
    1. Find the `filename` attribute and note its value for the answer.

    </details>

1.  <details>
    <summary>In these terraform labs, we have used multiple providers so far. But, what are providers?</summary>

    * Providers are _plugins_ for the `terraform` program.
    * `terraform init` downloads these based on the content of your configuration files.
    * Other terraform commands which process your configuration load these plugins and call the code within them to process each resource specific to a given provider.

    </details>

1.  <details>
    <summary>Which one is a valid sub-command of the <b>terraform providers</b> command?</summary>

    A sub-command is the value that comes next on the command line.

    To list valid sub-commands of `terraform providers` do

    ```bash
    terraform providers --help
    ```

    </details>

1.  <details>
    <summary>A new configuration directory /root/terraform-projects/provider has been created. We have already run the terraform init command.<br/>Now check the provider plugins that have been downloaded from the command line utility</summary>

    Running `terraform providers` without a sub-command will give us the answer.

    ```bash
    cd /root/terraform-projects/provider
    terraform providers
    ```

    </details>


