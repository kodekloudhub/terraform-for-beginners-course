# Lab: HCL Basics

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Navigate to indicated directory in Explorer pane

1.  <details>
    <summary>Which configuration block is defined in the main.tf file at the moment?</summary>

    > `module`

    </details>

1.  <details>
    <summary>What is the source of the module used in this configuration?</summary>

    > Public terraform registry

    You can verify by searhcing for it at https://registry.terraform.io

    </details>

1.  <details>
    <summary>What is the version of the module used?</summary>

    Inspect the `version` argument to the module block.

    </details>

1.  <details>
    <summary>How many required arguments does this module expect?</summary>

    Refer to the [documentation](https://registry.terraform.io/modules/terraform-aws-modules/iam/aws/latest/submodules/iam-user) and scroll down to the `inputs` section.

    <details>
    <summary>Reveal</summary>

    > `1`

    </details>

    </details>

1.  <details>
    <summary>Which argument is to be specified, just to create an IAM User with this module?</summary>

    From where you retrieved the previous answer in the documentatin, get the name of this required variable

    <details>
    <summary>Reveal</summary>

    > `name`

    </details>
    </details>

1.  <details>
    <summary>Now, update this module block that will allow it to create an IAM User called <b>max</b>.</summary>

    1. Replace the comment in `main.tf` with the appropriate argument

        <details>
        <summary>Reveal</summary>

        ```
        name = "max"

        </details>

    1. Init and plan

        ```bash
        cd /root/terraform-projects/project-sapphire
        terraform init
        terraform plan
        ```

    </details>

1.  <details>
    <summary>How many resources are set to be created in the execution plan ?</summary>

    Examine the output of the `terraform plan` you just ran.

    <details>
    <summary>Reveal</summary>

    > `3`

    </details>
    </details>

1.  <details>
    <summary>Which resources are set to be created?</summary>

    Given we know how many resources will be created from the answer of the previous question, only one of the given options cann possibly be correct! Plus they're all present in the plan output.

    <details>
    <summary>Reveal</summary>

    > `aws_iam_access_key`, `aws_iam_user` and `aws_iam_user_login_profile`

    </details>


    </details>

1.  <details>
    <summary>Why is the module creating additional resources, when only the name for creating an IAM User was defined in the main.tf file?</summary>

    Refer to the [documentation]https://registry.terraform.io/modules/terraform-aws-modules/iam/aws/latest/submodules/iam-user) and scroll down to the `inputs` section. Inspect the default values for arguments we did not provide.

    <details>
    <summary>Reveal</summary>

    > The three resources will be created by default as per the modiule configuration.

    </details>



    </details>

1.  <details>
    <summary>We only want to create the IAM User. Update the module block to only allow <b>create_user</b>. Disable <b>create_iam_access_key</b> and <b>create_iam_user_login_profile</b>.</summary>

    You should haqve esablished that there are some default `tru` boolean values for some arguments. These need to be explicitly set `false` to disable the subresource creation.

    1. Update the mdule resource accordingly

        <details>
        <summary>Reveal</summary>


        ```
        module "iam_iam-user" {
            source  = "terraform-aws-modules/iam/aws//modules/iam-user"
            version = "3.4.0"
            name                          = "max"
            create_iam_user_login_profile = false
            create_iam_access_key         = false
        }
        ```

        </details>


    1. Deploy

        ```bash
        cd /root/terraform-projects/project-sapphire
        terraform plan
        terraform apply
        ```

    </details>

