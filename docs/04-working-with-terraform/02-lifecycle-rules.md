# Lab: Lifecycle Rules

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>We have a directory created called /root/terraform-projects/project-mysterio. The main.tf file already has a couple of resource blocks.<br/>Which resource types do they use?</summary>

    1. Navigate to the indicated directory in the Explorer pane and open `main.tf`
    1. Remember which field of the `resouce` definition is the resource type

    </details>

1.  <details>
    <summary>Now, create these two resources that have been defined in this configuration file.</summary>

    In the terminal run the usual

    ```bash
    cd /root/terraform-projects/project-mysterio
    terraform init
    terraform plan
    terraform apply
    ```

    </details>

1.  <details>
    <summary>Which resource is created first in this case?</summary>

    > `string`

    Why? Because the `file` resource has an implicit dependency on `string` by way of the value of its `content` attribute.

    </details>

1.  <details>
    <summary>We have modified the resource configuration again. Run a terraform plan now. What would happen?</summary>

    ```bash
    cd /root/terraform-projects/project-mysterio
    terraform plan
    ```

    We see `Plan: 2 to add, 0 to change, 2 to destroy.`. This means both are being replaced

    </details>

1.  <details>
    <summary>Why is the string resource being re-created?</summary>

    Examine the plan output. There are two properties that say "forces replacement", however one is as a result of the other.

    > The value of `keepers` will be changed

    </details>

1.  Information only

1.  <details>
    <summary>Let's change the order in which the resource called string is recreated. Update the configuration so that when applied, a new random string is created first before the old one is destroyed.</summary>

    This is where lifecycle rules come in. We must add one to the string resource.

    1. Add the lifecycle rule within the `string` resource

        <details>
        <summary>Reveal</summary>

        ```
            lifecycle {
                create_before_destroy =  true
            }
        ```
        </details>

    1. Run `terraform apply`

    </details>

1.  <details>
    <summary>The resource block for the file resource has been updated! This will force the resource to be recreated during the next apply! But, before that, let's also add a lifecycle rule of create_before_destroy to this resource block.</summary>

    1. Add the same lifecycle block code to the `file` resource as you did for the `string` resource - copy/paste it.
    1. **Important**: Once the lifecycle rule has been added, only run the apply command once. We will learn why soon.
    1. Run `terraform apply`

    </details>

1.  <details>
    <summary>Great! We have now added the lifecycle rule and forced the resources to be created first and then destroyed.<br/>What is the id of the file resource we just created?</summary>

    1. Run `terraform show`
    1. Inspect the output to find the `id` of the `file` resource.

    </details>

1.  <details>
    <summary>Read the contents of the file /root/random_text manually. (Try opening with Visual Studio Code / cat command or a text editor)</summary>

    1. Look in the Explorer pane for this file.
    1. Er, it isn't there!

    </details>

1.  Information only

1.  <details>
    <summary>We have now wiped out the resources that were created in this configuration directory and updated the main.tf file.<br/>Now, it only contains a single random_pet resource called super_pet.<br/>Under which circumstances will a new pet id be created?</summary>

    Know that a new `id` for a resource is only generated if it is _replaced_, or it does not exist and will be created.

    Since we have input varaibles for both `length` and `prefix`, we can experiment with `terraform plan`. Examine the default values in `variables.tf`

    1. In terminal, navigate to project directoy.

        ```bash
        cd /root/terraform-projects/project-mysterio
        ```

    1. Try `length`. Use a value different from the default.

        ```
        terraform plan -var length=12
        ```

        Does this replace or create the resource?

    1. Try `prefix`. Use a value different from the default.

        ```
        terraform plan -var prefix=Dr
        ```

        Does this replace or create the resource?

    </details>

1.  <details>
    <summary>Now, update the configuration so that the resource super_pet is not destroyed under any circumstances with a terraform apply command.</summary>

    Another lifecycle rules here. We must add one to the `super_pet` resource.

    1. Add the lifecycle rule within the `super_pet` resource

        <details>
        <summary>Reveal</summary>

        ```
            lifecycle {
                prevent_destroy =  true
            }
        ```
        </details>

    1. Apply the configuration (it hasn't been applied yet), and plan a destroy

        ```
        cd /root/terraform-projects/project-mysterio
        terraform apply
        terraform plan -destroy
        ```

    </details>

