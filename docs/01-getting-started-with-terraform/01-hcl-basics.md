# Lab: HCL Basics

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Information only

1.  <details>
    <summary>Navigate to the directory /root/terraform-projects/HCL. There is a file present in this location. What is the file extension used by this file?</summary>

    1.  Open the VSCode terminal.
    1.  In the terminal, do

        ```
        cd /root/terraform-projects/HCL
        ls
        ```

        We see `main.tf`. The file name is `main` and the file extension is `.tf`

    </details>

1.  Information only

1.  <details>
    <summary>What is the resource type specified in this file?</summary>

    Inspect with VSCode:

    1. Click on `terraform-projects` in the explorer pane
    1. Click on the revealed `main.tf` to open the file
    1. Examine the `resource` line. The first argument is the `type` and the second argument is the `name`, therefore the answer is

        > `local_file`

    </details>

1.  <details>
    <summary>What is the resource name used for the local_file resource in this configuration file?</summary>

    Following on from the above, the answer is

    > `games`

    </details>

1.  <details>
    <summary>What is the name of the provider for which we are creating this resource?</summary>

    Know that the format of a resource type when decalring a resource is generally `provider_type`, therefore given `local_file` we have

    * `local` - name of the provider.
    * `file` - resource provided by the provider.

    So the answer is

    > `local`

    </details>

1.  <details>
    <summary>Which one of the below is not an example of an argument used within the resource block?</summary>

    Know that arguments are what appears between `{` and `}` in a resource block and are of the form `argument = value`

    This resource has two arguments: `filename` and `content` therefore the answer is the one that is not one of these two arguments

    > `resource_type = "local_file"`

1.  <details>
    <summary>If you run a terraform plan now? Would it work?</summary>

    > `No`

    Go ahead and try it in the terminal and note the errors.

    Why? We cannot plan or apply resources without first doing `terraform init`. For a new configuration, we must init the configuration to ensure all the correct providers are installed.

    </details>

1.  <details>
    <summary>Run a terraform init inside the configuration directory: /root/terraform-projects/HCL</summary>

    1. If the terminal is closed, reopen it
    1. Run the following commands

        ```bash
        cd /root/terraform-projects/HCL
        terraform init
        ```
    </details>

1.  <details>
    <summary>What was the version of the local provider plugin that was downloaded?</summary>

    Inspect the output from `terraform init`. It lists the names and versions of all providers that were initialized.

    </details>

1.  <details>
    <summary>Now, try to run a terraform plan.</summary>

    In the terminal run

    ```
    terraform plan
    ```

    This will either print an execution plan, or it will print a list of errors.

    </details>

1.  <details>
    <summary>Why did the command fail?</summary>

    Inspect the error produced when the command was run. If unsure, refer to the [documentation](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file).

    We can see that it is complaining about arguments.

    </details>

1.  <details>
    <summary>Which of the following is not a valid argument for the local_file resource?</summary>

    Check the `Unsupported argument` error in the plan output. The argument that is "not expected here" is the invalid argument.

    </details>

1.  <details>
    <summary>Fix the argument in the configuration file and then run a terraform plan followed by terraform apply to create the local_file resource called games.</summary>

    Check the `Missing required argument` error in the plan output. This gives you the correct name for the invalid argument.

    1. Edit `file` to be `filename`
    1. Run

        ```
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>What is the new argument that we have added?</summary>

    There are now three arguments to the resource. Two of them were there before (although the values were different), and one of them is new.

    > `sensitive_content`

    The values of sensitive fields do not show up in normal execution plans.

    </details>

1.  <details>
    <summary>If we run terraform plan or terraform apply now we see an error!<br/>Identify and fix the issue.</summary>

    You may ignore the deprecation warning.

    The error here is "Invalid attribute combination". This means we have specified more than one argument in a mutually exclusive argument group. The list of mutually exclusive arguments is provided in the message:<br/>
    `[content,sensitive_content,content_base64]`

    You are asked to ensure the file content does not show up in the execution plan, so you must remove the argument that is not sensitive, i.e. `content`

    Remove this, then run

    ```
    terraform plan
    terraform apply
    ```

    Unfortunately the sensitive content still shows up in the deprecation warning. Hashicorp should really fix that! It does not however show up in the execution plan itself.

    </details>

1. Information only

1.  <details>
    <summary>Finally, destroy this resource using terraform destroy.</summary>

    Run

    ```
    terraform destroy
    ```

