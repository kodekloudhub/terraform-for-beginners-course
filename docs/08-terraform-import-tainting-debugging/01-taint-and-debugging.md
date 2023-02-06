# Lab: Taint and Debugging

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>Which environment variable should be used to export the logs to a specific path?</summary>

    Refer to the [documentation](https://developer.hashicorp.com/terraform/internals/debugging)

    <details>
    <summmary>Reveal</summmary>

    > `TF_LOG_PATH`

    </details>


    </details>

1.  <details>
    <summary>Can you export the debug logs from terraform just by setting <b>TF_LOG_PATH</b> environment variable and providing a path as the value to this variable?</summary>

    Refer to the [documentation](https://developer.hashicorp.com/terraform/internals/debugging)

    <details>
    <summary>Reveal</summary>

    > `NO'

    This variable sets the path for log files to be written. It has no effect unless `TF_LOG` is also set.
    </details>


    </details>

1.  <details>
    <summary>We have a configuration directory called <b>/root/terraform-projects/ProjectA</b>. Enable logging with the log level set to <b>ERROR</b> and then export the logs the path <b>/tmp/ProjectA.log</b>.</summary>

    1.  Set the environment variables

        <details>
        <summary>Reveal</summary>

        ```bash
        cd /root/terraform-projects/ProjectA
        export TF_LOG=ERROR
        export TF_LOG_PATH=/tmp/ProjectA.log
        ```

        This variable sets the path for log files to be written. It has no effect unless `TF_LOG` is also set.

        </details>

    1.  Deploy

        ```
        terraform init
        terraform apply
        ```

    </details>

1.  <details>
    <summary>Which Log Level provides the most details when you run terraform commands?</summary>

    Refer to the [documentation](https://developer.hashicorp.com/terraform/internals/debugging)

    <details>
    <summmary>Reveal</summmary>

    > `TRACE`

    </details>


    </details>

1.  <details>
    <summary>Now navigate to /root/terraform-projects/ProjectB. We already have a main.tf file created for provisioning an AWS EC2 instance</summary>

    Apply the resource

    ```bash
    cd /root/terraform-projects/ProjectB
    terraform init
    terraform apply
    ```

    </details>

1.  <details>
    <summary>Now, try running a terraform plan again. What is the effect?</summary>

    ```bash
    terraform plan
    ```

    Note what will happen from the output of the above


    </details>

1.  <details>
    <summary>Why is the resource called ProjectB being replaced?</summary>

    Again, examine the plan output. The reason for replacement will be found there.

    <details>
    <summmary>Reveal</summmary>

    > `Reource is tainted`

    </details>
    </details>

1.  <details>
    <summary>Untaint the resource called ProjectB so that the resource is not replaced any more.</summary>

    <details>
    <summmary>Reveal</summmary>

    ```
    terraform untaint aws_instance.ProjectB
    ```

    You can run `terraform plan` again the verify.

    </details>


    </details>

