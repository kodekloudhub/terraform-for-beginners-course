# Lab: Resource Dependencies

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Which argument should be used to explicitly set dependencies for a resource?</summary>

    > `depends_on`

    </details>

1.  <details>
    <summary>Resource A relies on another Resource B but doesn't access any of its attributes in its own arguments. What is this type of dependency called?</summary>

    > `explicit dependency`

    This is so, because we haven't referred to any of Resource B's attributes. A dependency is explict when we make it so by use of `depends_on`.

    </details>

1.  <details>
    <summary>How do we make use of implicit dependency?</summary>

    > `reference expressions`

    Referencing another resource's attributes creates an implict dependency. This means that the other resource must be created first so that its attributes are known and can be read.

    </details>

1.  <details>
    <summary>In the configuration directory /root/terraform-projects/key-generator, create a file called key.tf ...</summary>

    * Resource Type: `tls_private_key`
    * Resource Name: `pvtkey`
    * algorithm: `RSA`
    * rsa_bits: `4096`

    1. Navigate to the given directory in Explorer pane and add `key.tf`
    1. Add the resource as requested

        [Documentation](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key)

        <details>
        <summary>Reveal</summary>

        ```
        resource "tls_private_key" "pvtkey" {
            algorithm = "RSA"
            rsa_bits = 4096
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/key-generator
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  Information only

1.  <details>
    <summary>Now, let's use the private key created by this resource in another resource of type local file. Update the key.tf file</summary>

    * Resource Name: `key_details`
    * File Name: `/root/key.txt`
    Content: use a reference expression to use the attribute called `private_key_pem` of the `pvtkey` resource.

    1. Add the resource as requested

        [Documentation](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key)

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "key_details" {
            filename = "/root/key.txt"
            content = tls_private_key.pvtkey.private_key_pem
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/key-generator
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>Now destroy these two resources.</summary>

    ```bash
    terraform destroy
    ```

    </details>

1.  Information only - navigate to indicated directory in Explorer pane.

1.  <details>
    <summary>Within this directory, create two local_file type resources in main.tf file.</summary>

    * Resource 1:
        * Resource Name: `whale`
        * File Name: `/root/whale`
        * content: `whale`
    * Resource 2:
        * Resource Name: `krill`
        * File Name: `/root/krill`
        * content: `krill`

    Resource called whale should depend on krill but do not use reference expressions.

    This means we need to create an _explicit_ dependency _on_ `whale` _to_ `krill` which will ensure that `krill` is created before `whale`

    1. Add file `main.tf`
    1. Declare resources

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "whale" {
            filename = "/root/whale"
            content = "whale"
            depends_on = [ local_file.krill ]
        }

        resource "local_file" "krill" {
            filename = "/root/krill"
            content = "krill"
        }
        ```

        Note that I have deliberately put `whale` first in the file. The ordering of the resources in the file does not matter. The dependencies, either implicit or explicit is what determines the order of creation.

        Note also that `depends_on` is a list argument, as a resource can depend on one or more other resources.

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects//explicit-dependency
        terraform init
        terraform plan
        terraform apply
        ```


    </details>

