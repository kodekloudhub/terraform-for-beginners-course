# Lab: Datasources

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>A data source once created, can be used to create, update, and destroy infrastructure?</summary>

    > `FALSE`

    A datasource can only read resource data and use that information within terraform.

    </details>

1.  <details>
    <summary>A data source can be created using the <b>data</b> block.</summary>

    > 'TRUE'

    </details>

1.  <details>
    <summary>A new configuration directory has been created at <b>/root/terraform-projects/project-lexcorp</b>. A data source block is defined in the <b>main.tf</b> file to read the contents of an existing file.<br/>There is also an output variable that uses reference expression to print the file content using this data source. However, there is something wrong!<br/>Troubleshoot and fix the issue.</summary>

    1. We can immediately see the first issue based on the onswer to the previous question. There is no such block as `datasource`. Correct this.
    1. Let's see if we fixed it

        ```bash
        cd /root/terraform-projects/project-lexcorp
        terraform init
        terraform plan
        ```

    1. There is another error at line 2

        `A data resource "local_file" "content" has not been declared in the root
        module.`

        The value reference at this line is incorrectly looking for

        ```
        data "local_file" "content" { ... }
        ```

        so there is a mistake in the reference expression. A reference expression for a data block is:

        `data.type.name.attribute`

        We know from previous labs that the `local_file` resource has an _attribute_ `content`, therefore we are missing the resource name from the reference. Correct it:

        > `data.local_file.os.content`

    1. Deploy

        ```
        terraform plan
        terraform apply
        ```


    </details>

1.  Information only.

1.  <details>
    <summary>We have now created a new configuration file called <b>ebs.tf</b> within the same configuration directory we have been working on.<br/>What is the resource type that we are working with here?</summary>

    The first property of the `data` block is the resource type, just as it is for `resource`

    > `aws_ebs_volume`

    </details>

1.  <details>
    <summary>Once this data source is created, how do we fetch the <b>Volume Id</b> for the resource that is created in AWS?</summary>

    Refer to the [documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ebs_volume) and look in the Attributes Reference section.

    Choose the correct attribute from those given. Attribute names are case sensistive.

    </details>

1.  <details>
    <summary>Another file called <b>s3.tf</b> has now been created. It too has a data source that will be used to read data of an existing s3 bucket.<br/>However, there is a mistake in the argument used. What is wrong here?</summary>

    This data source has an argument, which is the name of the bucket to get information on.

    Refer to the [documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3_bucket) and look in the Argument Reference section. You will see that there is only one argument, and it's not in agreement with what is in `s3.tf`, therefore the answer must be

    > `bucket_name is not a valid argument`


    </details>

1.  Information only.

