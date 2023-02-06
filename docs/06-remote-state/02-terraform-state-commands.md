# Lab: Terraform State Commands

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>We have created a few resources in the configuration directory <b>/root/terraform-projects/project-anime</b>. Inspect it.<br/>Which of the following resources names are not part of the terraform state?</summary>

    1. Navigate to indicated directory in Explorer pane.
    1. Open the `terraform.tfstate` file and examine the resources. There are four.
    1. Find the one that isn't in there

        <details>
        <summary>Reveal</summary>

        > `super_pets`

        </details>
    </details>

1.  <details>
    <summary>Which command would you use to show the attributes of the resource called classics stored in the terraform state?</summary>

    <details>
    <summary>Reveal</summary>

    ```bash
    cd /root/terraform-projects/project-anime
    terraform state show local_file.classics
    ```

    We can pick individual resources by using the same reference syntax to identify a resource: `resource_type.resource_name`

    </details>

    </details>

1.  <details>
    <summary>What is the value of the attribute called id that is created for the local file resource called top10?</summary>

    We can again use `terrform state show` to see the attribute details.

    <details>
    <summary>Reveal</summary>

    ```bash
    cd /root/terraform-projects/project-anime
    terraform state show local_file.top10
    ```

    </details>

    </details>

1.  <details>
    <summary>We no longer wish to manage the file located at <b>/root/anime/hall-of-fame.txt</b> by Terraform. Remove the resource responsible for this file completely from the management of terraform.</summary>

    Determine which resource represents the given file and use the `terraform state rm` command. Don't forget to also remove it from the configuration file, or terraform will attempt to recreate it at the next apply.

    <details>
    <summary>Reveal</summary>

    ```bash
    cd /root/terraform-projects/project-anime
    terraform state rm local_file.hall_of_fame
    ```

    </details>


    </details>

1.  <details>
    <summary>Now navigate to the directory <b>/root/terraform-projects/super-pets</b>. Just like the previous configuration directory, we have already created the resource. Inspect the configuration and identify the only resource type used.</summary>

    1. Navigate to indicated directory in Explorer pane.
    1. Inspect `main.tf` - you will see two resources of the same _type_.

    </details>

1.  <details>
    <summary>Within this configuration the terraform state commands are working (Try it!) but there is no terraform.tfstate file present!<br/>What is the reason for this behavior?</summary>

    Examine `teraform.tf`. What do you notice about the backend configuration?

    <details>
    <summary>Reveal</summary>

    > We are using remote state

    Yes, the backend is in S3. `terrafrom state` is interrogating the remote state directly.

    </details>

    </details>

1.  <details>
    <summary>What is the id of the random_pet resource called super_pet_2 in the state file?</summary>

    We can again use `terrform state show` to see the attribute details.

    <details>
    <summary>Reveal</summary>

    ```bash
    cd /root/terraform-projects/super-pets
    terraform state show random_pet.super_pet_2
    ```

    Inspect the `id`

    </details>

    </details>

1.  <details>
    <summary>Rename the resource from super_pet_1 to ultra_pet.</summary>

    Use `terraform state mv` to do this, then update it in `main.tf` or the next apply will put if back as it was.

    <details>
    <summary>Reveal</summary>

    ```bash
    cd /root/terraform-projects/super-pets
    terraform state mv random_pet.super_pet_1 random_pet.ultra_pet
    ```

    </details>

    </details>

