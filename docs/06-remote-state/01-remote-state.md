# Lab: Remote State

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Navigate to the indicated directory in the Explorer pane.

1.  <details>
    <summary>First, create a simple configuration to create a <b>local_file</b> resource within the directory called <b>RemoteState</b>. The resource block should be created inside the <b>main.tf</b> file.</summary>

    Specification

    * Resource Name: `state`
    * filename: `/root/<variable local-state>`
    * content: "`This configuration uses <variable local-state> state"`
    * Use the variable called local-state in the `variables.tf` file which is already created for you and make use of variable interpolation syntax `${..}`.

    1. In the indicated directory add `main.tf`
    1. Create the resource as directed

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "state" {
            filename = "/root/${var.local-state}"
            content  = "This configuration uses ${var.local-state} state"
        }
        ```
        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/RemoteState
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>Has a state file been created after you run terraform apply?</summary>

    > `YES`

    </details>

1.  <details>
    <summary>What is the name of the state file created for this configuration?</summary>

    > `terraform.tfstate`

    </details>

1. Information only. Explore the minio UI.

1.  <details>
    <summary>We have already created an s3 bucket that we will use to store the remote state. From the Minio Browser, identify the name of this bucket.</summary>

    > `remote-state`

    </details>

1.  <details>
    <summary>Before we add the configuration for the s3 backend, let's first change the local file resource. Change the variable used to <b>remote-state</b> instead of <b>local-state</b>.</summary>

    1. In `main.tf` replace all occurrences of "local-state" with "remote-state".<br/>You can use `CTRL+H` to bring up find/replace.

    </details>

1.  <details>
    <summary>Great! Now, let us configure the remote backend with s3. Add a terraform block in a new file called terraform.tf with the following arguments:</summary>

    Arguments:

    * bucket: `remote-state`
    * key: `terraform.tfstate`
    * region: `us-east-1`

    1. Add new file `terraform.tf`
    1. Configure the backend as requested ([documentation](https://developer.hashicorp.com/terraform/language/settings/backends/s3))

        <details>
        <summary>Reveal</summary>

        ```
        terraform {
            backend "s3" {
                key = "terraform.tfstate"
                region = "us-east-1"
                bucket = "remote-state"
            }
        }
        ```
        </details>

    Do not run `terraform init` yet! Since we are making use of minio we also have to add a couple of additional arguments to get this to work!

    We will do that in the next step. When using the regular s3 service from AWS the above arguments should be sufficient to configure remote state.
    </details>

1.  Information only. Examine the changes we have made to `terraform.tf`

1.  <details>
    <summary>Try running terraform apply, are you able to do it? If not why?</summary>

    ```bash
    cd /root/terraform-projects/RemoteState
    terraform apply
    ```

    You can see from the error that it requires to be initiailzed. When changing backend configuration this is always required.

    </details>

1.  <details>
    <summary>Run terraform init in our configuration directory now.</summary>

    ```
    terraform init
    ```

    Answer `yes` to the prompt, to allow terraform to copy the state file to the new backend.

    The local copy of `terraform.tfstate` may now be deleted.

    In the minio UI, go to Object Browser and select the bucket. You will see `terraform.tfstate` stored there.

    </details>

