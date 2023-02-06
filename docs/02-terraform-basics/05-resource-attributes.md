# Lab: Resource Attributes

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Information only

1.  <details>
    <summary>What is the resource_type of the resource that's currently defined in the main.tf file?</summary>

    Refer to the [documentation](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/static)

    </details>

1.  Information only

1.  <details>
    <summary>Which of the following attributes are exported by the time_static resource?</summary>

    Refer to the [documentation](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/static), and scroll down to the `Read-Only` section below `Schema`. Compare what is there with the given answers and select the correct one.

    Read-only attributes are exported by a resource, and can be referenced from other resources.

    Note that you cannot assign a value to a read-only attribute in a `.tf` configuration file.

    </details>

1.  <details>
    <summary>How do we refer to the attribute called id using a reference expression?</summary>

    Know that a resource attribute reference is of the form<br/>`resource_type.resource_name.attribute_name`

    > `time_static.time_update.id`

    </details>

1.  <details>
    <summary>Now, update the main.tf file and add a new local_file resource called time with the following requirements</summary>

    * filename: `/root/time.txt`
    * content: `Time stamp of this file is <id from time_update resource>`

    For this, we need to add a `local_file` resource, like in previous labs. However this time, we need to refer to the ID of the `time_static` resource for the content, so we'll be using the expression we determined in Q5.

    1. Update `main.tf` and add new resource

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "time" {
            filename = "/root/time.txt"
            content = "Time stamp of this file is ${time_static.time_update.id}"
        }
        ```
        </details>

    1. Deploy the resources from the terminal

        ```bash
        cd /root/terraform-projects/project-chronos
        terraform init
        terraform plan
        terraform apply
        ```
    </details>

1.  <details>
    <summary>What is the attribute called id that is created for the local file resource called time?</summary>

    Make use of the terraform show command and identify the attribute values.

    ```bash
    cd /root/terraform-projects/project-chronos
    terraform show
    ```

    This will print out the resources that were defined, along with the current values for all their attributes. Find the `id` attribute for the `local_file` resource.

    </details>

1.  <details>
    <summary>What is the attribute called rfc3339 that is created for the time_static resource called time_update?</summary>

    Refer again to the output from `terraform show`. Find the `rfc3339` attribute for the `time_static` resource.

    </details>

