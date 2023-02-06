# Lab: Variables

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Which of the following is not a valid variable type?</summary>

    Refer: https://developer.hashicorp.com/terraform/language/expressions/types

    > `item`

    </details>

1.  <details>
    <summary>Which one of the below is not a valid data type in terraform?</summary>

    Referring to the same documentation...

    > `array`

    In terraform, we refer to array-like types as `list`

    </details>

1.  <details>
    <summary>Navigate to the directory /root/terraform-projects/variables. Which type does the variable called number belong to?</summary>

    1. In the Explorer pane, navigate the the indicated file and click to open.
    1. find `variable "number"` and examine the `type` attribute

    > `bool`

    </details>

1.  <details>
    <summary>How would you fetch the value of the key called slow from the variable called hard_drive in a terraform configuration?</summary>

    Examine the `variables.tf` file. See the variable `"hard_drive"`. This is a map type with strings as keys. It is also initialized with a default value which has the two key-value pairs `slow` and `fast`.

    Know that when referencing a variable, we must use the prefix `var.`, and when accessing a key in a map we use `[]` with the key name within.

    > `var.hard_drive["slow"]`

    </details>

1.  <details>
    <summary>What is the index of the element called Female in the variable called gender?</summary>

    Examine the variable `"gender"` in the file. It is a list of strings with a default value.

    Know that when indexing a list, the first element is at index zero.

    > `1`


    </details>

1.  <details>
    <summary>What is the type of variable called users?</summary>

    This is the same as Q3. Examine the `type` attribute of the `users` variable.

    > `set(string)`

    </details>

1.  <details>
    <summary>However, this variable has been defined incorrectly! Identify the mistake.</summary>

    Know that the definition of a `set` is that its values must be unique. Examine the values closely with this in mind.

    > `duplicate elements`

    </details>

1.  Information only.

1.  <details>
    <summary>What is the value for the argument called content used in the resource block for the resource jedi?</summary>

    Look in `main.tf`. Identify the value of the attribute

    > `phanius`

    </details>

1.  <details>
    <summary>Now, let's update this resource and add variables instead. Use the default value declared in the variable called jedi.</summary>

    1. Inspect `variables.tf`. Find the `jedi` variable.
    1. This variable, like the `hard_drive` variable is a map keyed with strings. Therefore we need to make expressions like the answer to Q4.
    1. Modify the `jedi` resource accordingly

        <details>
        <summary>Reveal</summary>

        ```
        resource "local_file" "jedi" {
            filename = var.jedi["filename"]
            content = var.jedi["content"]
        }
        ```

        Ingnore it if you get a red underline in the file. Possible bug in the VSCode terraform plugin.

    1. Deploy the resource

        ```bash
        cd /root/terraform-projects/variables
        terraform init
        terraform plan
        terraform apply
        ```

    </details>


