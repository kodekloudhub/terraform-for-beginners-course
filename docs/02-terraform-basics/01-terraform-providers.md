# Lab: Terraform Providers

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>We have a new configuration directory located at the path /root/terraform-projects/things-to-do. Inspect this directory and find out the number of providers initialized within this directory.</summary>

    Konw that when a configuration is initialized with `terraform init`, a subdirectory `.terraform` is created by the init process.

    Inspect `/root/terraform-projects/things-to-do` directory as directed and look for `.terraform` there. If it doesn't exist, then no providers are initialized.

    </details>

1.  <details>
    <summary>How about now? How many provider plugins are installed in this configuration directory?</summary>

    The `.terraform` directory now exists. Expand this in the Explorer pane of the VSCode editor, and see that there is an entry<br/>`registry.terraform.io/hashicorp/local/2.3.0/linux_amd64/`<br/>This represents a single provider plugin.

    </details>

1.  <details>
    <summary>How many configuration files exist in the directory: /root/terraform-projects/things-to-do ?</summary>

    Configuration files are genrally files with a `.tf` file extension. Count them.

    </details>

1.  <details>
    <summary>How many resources are configured in this configuration directory?</summary>

    Count all the resource blocks used.

    We can see two resources: `things-to-do` and `more-things-to-do`

    </details>

1.  <details>
    <summary>What is the version of the plugin for the local provider that has been downloaded for this configuration?</summary>

    Referring back to what we did io Q2, we can see the version as part of the path<br/>`registry.terraform.io/hashicorp/local/2.3.0/linux_amd64/`

    </details>

1.  <details>
    <summary>Now, go ahead and create these resources using terraform!</summary>

    Run the following

    ```bash
    cd /root/terraform-projects/things-to-do
    terraform plan
    terraform apply
    ```

    Note that we didn't do `terraform init` here. That was done for you by the setup phase of Q2.

    </details>

1.  Information only

1.  <details>
    <summary>How many resources are configured within this configuration directory?</summary>

    1. Navigate to `christmas-wishlist` in the Eplorer pane.
    1. Open each of the `.tf` files in turn
    1. Count the resource blocks in each file.
    1. Sum the results

    </details>

1.  <details>
    <summary>What is the filename that will be created by the resource called cyberpunk?</summary>

    Check the value for `filename` argument in `cyberpunk.tf`

    </details>

1.  <details>
    <summary>Create a new configuration file within the same directory called xbox.tf. This file should make use of the same local_file resource type with the below requirements:</summary>

    * Resource name: `xbox`
    * filename: `root/xbox.txt`
    * content: `Wouldn't mind an XBox either!`

        <details>
        <summary>Reveal code</summary>

        ```hcl
        resource "local_file" "xbox" {
            filename = "/root/xbox.txt"
            content = "Wouldn't mind an XBox either!"
        }
        ```
        </details>

    Now run the following in the terminal

    ```bash
    cd /root/terraform-projects/christmas-wishlist
    terraform init
    terraform plan
    terraform apply
    ```

    </details>

1.  <details>
    <summary>Now, navigate to the directory /root/terraform-projects/provider-a. We have downloaded a plugin in this directory. Identify the name and type of provider.</summary>

    1. You can look in the `.terraform` directory beneath `provider-a` in the Explorer pane. Here we see `linode`
    1. Go to https://registry.terraform.io and search for `linode`, then click on the result.
    1. Now we see that the provder is "by: linode" and that linode is a partner. This means it is "verified". Official providers are "by: Hashicorp".

    </details>

1.  <details>
    <summary>Now, navigate to the directory /root/terraform-projects/provider-b. We have downloaded a plugin in this directory. Identify the name and type of provider.</summary>

    Just as for the previous question, find the provider on terraform registry. Since it is neither by Hashicorp, nor by Ansible themselves and marked as Partner, it is therefore a community provider.

    Anyone with golang programming skills can create a community provider and publish it to the registry.

    </details>

