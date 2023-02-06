# Lab: count and for_each

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>A new configuration directory has been created at <b>/root/terraform-projects/project-shade</b>. Inspect it. How many files will be created by this configuration?</summary>

    1. Navigate to the indicated directory in the Explorer pane and open `main.tf`
    1. Count the `local_file` resources


    </details>

1.  <details>
    <summary>Now add a count argument to create 3 instances of this resource.</summary>

    1. Add count to the resource

        ```
        count = 3
        ```

    1. Deploy

        ```bash
        cd /root/terraform-projects/project-shade
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>The resource <b>local_file.name</b> is now created as a:</summary>

    `count` on a resource generates mutiple instances of the resource as a `list`

    </details>

1.  <details>
    <summary>what is the id for the resource element at index 1?</summary>

    Run `terraform show` or `terraform state show local_file.name[1]` in the terminal

    </details>

1.  <details>
    <summary>How many files were actually created when apply was run?</summary>

    Since we did not do anything with the filename argument related to the count, terraform ended up writing the _same_ file three times, so the answer is

    > `1`

    </details>

1.  <details>
    <summary>We have now created a <b>variables.tf</b> file in the same configuration directory. Update the <b>main.tf</b> file to make use of the list type variable defined for the filename argument.</summary>

    * Use `count` to loop through all the elements of this list and do not use hard-coded values.
    * Use the variable called `content` for the argument called sensitive_content.

    Inspect `variables.tf` and note that the `list` variable is called `users`. This implies that we're going to greate one file per user in this list. The variable has no default value, meaning that eventually the values will come from elsewhere.

    There's a few things we will need to do here.

    1. Set the `count` argument from the length of the list (which will be known at plan time) using the [length()](https://developer.hashicorp.com/terraform/language/functions/length) function.
    1. Set the `filename` argument using the current value of the counter, which is obtained via the [index](https://developer.hashicorp.com/terraform/language/meta-arguments/count#the-count-object) attribute of `count`. This index must index into the `users` list.
    1. Set the `content` argument as directed

    Update the resource accodringly:

    <details>
    <summary>Reveal</summary>

    ```
    resource "local_file" "name" {
        filename = var.users[count.index]
        sensitive_content = var.content
        count = length(var.users)
    }
    ```

    </details>


    </details>

1.  <details>
    <summary>We have reverted back to the old configuration file and cleaned up the resources created so far. A variable called users now has default values added to it.<br/>What type of variable is it?</summary>

    Inspect `variables.tf` and check the value of the `type` attribute.

    </details>

1.  <details>
    <summary>Can the same elements in this list be used as it is for a set instead?</summary>

    Know that a `set` must have unique values. Are all the values in this list unique?

    </details>

1.  <details>
    <summary>Let's do the same exercise as before but this time we will make use of the for_each meta argument to create the files in this configuration.<br/>Just like before don't use any hard-coded values.</summary>

    * Use `for_each` to loop through the list type variable called `users`.
    * Use the variable called `content` as the value of the argument sensitive_content.

    Know that `for_each` requires a `set` not a `list`, therefore we must convert the list to a set using the [toset()](https://developer.hashicorp.com/terraform/language/functions/toset). This function will also remove any dupicates from the list it is given. Doing it this way satifies the condition that we must not change any hard coded value - just in case you were tempted to edit `variables.tf` and make that list into a set!

    1. Update resource accordingly

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "name" {
            for_each = toset(var.users)
            filename = each.value
            sensitive_content = var.content
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/project-shade
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>The resource called name is now created as:</summary>

    > `map`

    Why? Because now the resources are keyed by the values that were generated from the list by `for_each`, rather than by a numeric index as generated by `count`

    </details>

1.  <details>
    <summary>The resource address with the filename - <b>/root/user11</b> is now represented as:</summary>

    AS we found out, the resources are now generated as a map keyed by the filenames extracted from the users list, therefore the answer must be

    > `local_file.name["/root/user11"]`

    </details>

