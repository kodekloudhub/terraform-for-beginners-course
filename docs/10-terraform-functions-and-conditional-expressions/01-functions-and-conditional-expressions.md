# Lab: Functions and Conditional Expressions

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Information only

1.  <details>
    <summary>What value does floor(10.9) produce?</summary>

    Know that `floor` rounds_down_ to the nearest integer

    <details>
    <summary>Reveal</summary>

    > `10`

    </details>


    </details>

1.  <details>
    <summary>What value does <b>title("user-generated password file")</b> produce?</summary>

    Know that `title` converts its input to Title Case

    <details>
    <summary>Reveal</summary>

    > `User-Generated Password File`

    </details>

    </details>

1.  <details>
    <summary>Which type of variable does the function <b>lookup</b> work with?</summary>

    <details>
    <summary>Reveal</summary>

    > `map`

    We _lookup_ a key in a map to find its value, and provide a default if the key does not exist.

    </details>

    </details>

1.  <details>
    <summary>What type of variable is cloud_users?</summary>

    1. Navigate to `/root/terraform-projects/project-sonic` in the Explorer pane
    1. Inspect `variables.tf` to find the answer

        <details>
        <summary>Reveal</summary>

        > `string`

        </details>

    </details>

1.  <details>
    <summary>This variable contains the names of the developers for project-sonic with the names separated by a <b>:</b>.<br/>Using this variable and the <b>count</b> meta-argument, create IAM users for all developers. Write the resource block in the <b>main.tf</b> file.</summary>

    Convert this variable from a string to a list. Do not change the variable defined in `variables.tf`.

    We have some work to do here, combining what has been learned in several previous course sections! We are going to draw upon the following

    * `aws_iam_user` resource from the Terraform with AWS section
    * `count` from the Working with Terraform section
    * A function that will split a string into a list on a given separator, which we may need to use more than once
    * A function that gives us the length of a list.
    <br/><br/><br/>
    * Documentation for [functions](https://developer.hashicorp.com/terraform/language/functions)
    * Documentation for [aws_iam_user](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user)
    <br/><br/>
    Hint: The resource created at the end of the [IAM with Terraform](../05-terraform-with-aws/02-iam-with-terraform.md) lab is very similar to what is required here!

    1. Determine the function required to split a string given a separator

        <details>
        <summary>Reveal</summary>

        > [split](https://developer.hashicorp.com/terraform/language/functions/split)

        Therefore to get a list from the `cloud_users` variable, it would be

        ```
        split(":", var.cloud_users)
        ```

        </details>

    1. Determine how we will set the `count` within the resource.

        We have the list as the expression defined above. Getting the length of this list will give us `count` for the number of users to create.

        <details>
        <summary>Reveal</summary>

        ```
        count = length(split(":", var.cloud_users))
        ```

        </details>

    1. Determine how we will set each user's name as the count is iterated. Remember the `index` property of `count`

        <details>
        <summary>Reveal</summary>

        We have to evaluate the split here as well

        ```
        name = split(":",var.cloud_users)[count.index]
        ```

        </details>

    1. Put this all together into a resource block

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_iam_user" "cloud" {
            name = split(":",var.cloud_users)[count.index]
            count = length(split(":",var.cloud_users))
        }
        ```

        </details>

    1. Deploy

        ```
        cd /root/terraform-projects/project-sonic
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>What is the name of the IAM User that is created at the Index 6, of the IAM User at address <b>aws_iam_user.cloud</b>?</summary>

    There are multiple ways to do this, not least simply inspecting the output of the apply, but you have been asked to use the console

    <details>
    <summary>Reveal</summary>

    ```
    terraform console
    aws_iam_user.cloud[6].name
    ```

    --- OR ---

    ```
    echo 'aws_iam_user.cloud[6].name' | terraform console
    ```

    > `braja`

    </details>

    </details>

1.  <details>
    <summary>Locate the index of the element called <b>oni</b> in the variable called <b>sf</b>.</summary>

    Find a function that return the index of an element in a list by value, and test it in the console.

    <details>
    <summary>Reveal</summary>

    The [index](https://developer.hashicorp.com/terraform/language/functions/index_function) function does this.

    ```
    terraform console
    index(var.sf, "oni")
    ```

    --- OR ---

    ```
    echo 'index(var.sf, "oni")' | terraform console
    ```

    > `7`

    </details>
    </details>

1.  <details>
    <summary>What type is the variable called media?</summary>

    Inspect `variables.tf` again to find the answer

    <details>
    <summary>Reveal</summary>

    > `set(string)`

    </details>
    </details>

1.  <details>
    <summary>We have now, updated the main.tf in this configuration directory and added a new resource block to create a S3 bucket called <b>sonic-media</b>. Create an additional resource called <b>upload_sonic_media</b> to upload the files listed in the variable called <b>media</b> to this bucket.</summary>

    Use the following specifications

    * Use the `for_each` meta-argument to upload all the elements of the `media` variable.
    * bucket: Use reference expression to the bucket `sonic-media`.
    * source: Each element in the variable called `media`.
    * key: Should be the name of the files being uploaded (minus the path component). For example, `eggman.jpg`, `shadow.jpg` etc.
    * Do not alter the variables!

    <br/>
    <br/>
    Refer back to the lab on S3 to find the resource used to upload files. Also refer to the lab/lectures on `for_each`.<br/><br/>

    Note also that the keys in the bucket need to be the _filename_ only, so we will need another function to strip off the `/media/` part of the pathname to get only the filename. There are two possible functions that can be used. One is to manipulate the string itself, and the other function we can use which is specifically for this purpose and would work if the paths weren't all `/media/` can be found in the Filesystem Functions in the documentation.

    <details>
    <summary>Reveal</summary>

    To upload files to buckets, we use [aws_s3_object](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3_object).

    Solving the requirements one at a time

    1.  Iterate the `media` variable

        ```
        for_each = var.media
        ```

    1.  Use reference expression for the bucket

        ```
        bucket = aws_s3_bucket.sonic_media.id
        ```

    1.  `source`, i.e. the file to be uploaded is each element of `media`

        ```
        source = each.value
        ```

    1.  Get the file _name_ of the the source file. As mentioned, there's two ways

        1. String chopping to retain only the part of the string _after_ `/media/` which is character position 7 onwards. We can use [substr](https://developer.hashicorp.com/terraform/language/functions/substr) to do this, setting the `length` argument to something big enough to get the longest string

            ```
            key =  substr(each.value, 7, 20)
            ```

        However that's messy and not safe.

        1. Use the [basename](https://developer.hashicorp.com/terraform/language/functions/basename) function that knows how to extract the filename and extension from a file path

            ```
            key =  basename(each.value)
            ```

    1.  Put it all together

        ```
        resource "aws_s3_object" "upload_sonic_media" {
            bucket = aws_s3_bucket.sonic_media.id
            key = basename(each.value)
            source = each.value
            for_each = var.media
        }
        ```

    1.  Deploy

        ```
        terraform plan
        terraform apply
        ```

    </details>


    </details>

1.  Information only. Navigate to indicated directory in Explorer pane.

1.  <details>
    <summary>What is the value of the variable called small?</summary>

    Inspect `variables.tf`

    <details>
    <summary>Reveal</summary>

    > `t2.nano`

    </details>
    </details>

1.  <details>
    <summary>What is the current value for the variable called name?</summary>

    Inspect `variables.tf`

    <details>
    <summary>Reveal</summary>

    This variable has no default. It is therefore

    > `undefined`

    </details>

    </details>

1.  <details>
    <summary>Create an EC2 Instance with the resource name <b>mario_servers</b>.</summary>

    Use the following specifications:

    * AMI: Use variable called `ami`.
    * Tags: Create a tag with key Name and value set to the variable called `name`.
    * Instance_type: Use a conditional expression so that - If the instance is created with a tag `Name = "tiny"`, it should use the variable called small else the variable called `large`.
    * We will supply the variable called name using the `-var` command line flag.

    <br/>

    You will need to make use of a conditional to set the instance type. We do not know what the `-var` will be set to by the question's validation script! Remember that conditionals have the form:<br/>`logical_expression ? value_when_true : value_when_false`

    <details>
    <summary>Reveal</summary>

    ```
    resource "aws_instance" "mario_servers" {
        ami = var.ami
        instance_type = var.name == "tiny" ? var.small : var.large
        tags = {
            Name = var.name
        }
    }
    ```

    Now run `terraform init` _only_. The validation will create the resource using the `-var` value of its choice.

    </details>



    </details>

