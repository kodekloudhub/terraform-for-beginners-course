# Lab: AWS CLI and IAM

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Information only

1.  <details>
    <summary>What is the exact version of the CLI installed on the iac-server?</summary>

    Run the following

    ```
    aws --version
    ```

    </details>

1.  <details>
    <summary>Which command should be used to interact with Identity and Access Management in AWS using the CLI?</summary>

    If you don't already know the answer you can try each of the given answers, e.g<br>
    `aws identity help` until you find the one that does't report "aws: error: argument command: Invalid choice".

    </details>

1.  <details>
    <summary>Which subcommand with iam can be used to list all the users created in aws?</summary>

    As with the previous question, try e.g. `aws iam user-list help` until you don't get "aws: error: argument operation: Invalid choice"

    </details>

1.  <details>
    <summary>Now, let's learn how to make use of the mocking framework used in the labs.<br>Run: <b>aws iam list-users</b><br/>Does it work?</summary>

    > `No`


    </details>

1.  <details>
    <summary>Now, run the same command but with the <b>--endpoint http://aws:4566<b> as the option</summary>

    ```
    aws --endpoint http://aws:4566 iam list-users
    ```

    We need this argument as the default for `aws` command is to contact the _real_ AWS. In these labs, we use an AWS simulator which is running in a docker container. We use `--endpoint` to redirect aws commands to that container.

    </details>

1.  <details>
    <summary>How many IAM Users do you see listed now?</summary>

    From the output of the command you ran in the previous quesiton, you can see that `Users` is a JSON array containing two users.

    </details>

1.  <details>
    <summary>Now let's add a few more users! To add more users, we need to make use of the create-user sub-command.<br/>However, we also need to pass in a mandatory option with this command for it to work?<br/>Which option should we use?</summary>

    Refer to the [documetnation](https://docs.aws.amazon.com/cli/latest/reference/iam/create-user.html). Mandatory options are the ones _not_ enclosed in square brackets in the Synopis paragraph, which lists the sub-command first (`create-user`) then the options.

    </details>

1.  <details>
    <summary>Create a new user called <b>mary</b> using the AWS CLI.</summary>

    ```
    aws --endpoint http://aws:4566 iam create-user --user-name mary
    ```

    </details>

1.  <details>
    <summary>Now, inspect the newly created user mary and find out its <b>ARN</b> (Amazon Resource Name).</summary>

    This information can be found from the output of the previous command to create the user, or you can run the `list-users` CLi command again.

    </details>

1.  <details>
    <summary>What is the default region that has been configured for use with the AWS CLI?</summary>

    AWS CLI configuration is located in the logged-in user's home directory under `.aws/config`

    ```bash
    cat ~/.aws/config
    ```

    Find the `region` setting.

    Note that `~` is an alias for "my home directory"

    </details>

1.  <details>
    <summary>What is the <b>aws_access_key_id</b> used in the configuration?</summary>

    AWS CLI credentials are located in the logged-in user's home directory under `.aws/credentials`

    ```bash
    cat ~/.aws/concredentialsfig
    ```

    Find the `aws_access_key_id` setting.

    Note that `~` is an alias for "my home directory"

    </details>

1.  <details>
    <summary>What is the value of <b>aws_secret_access_key</b> used?</summary>

    From the output of the command you ran in Q12, find the `aws_secret_access_key` setting.

    </details>

1.  <details>
    <summary>Now that we have a few users created, let's grant them privileges. Let's start with <b>mary</b>, grant her full administrator access by making use of the policy called <b>AdministratorAccess</b>.</summary>

    * Make use of the subcommand `attach-user-policy`.
    * The ARN of the AdministratorAccess policy is `arn:aws:iam::aws:policy/AdministratorAccess`.

    1. Look up the sub-command in the [documentation](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-user-policy.html)
    1. Note the two required options. We need to use both of them.

    ```
    aws --endpoint http://aws:4566 iam attach-user-policy \
        --user-name mary \
        --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
    ```

    </details>

1.  <details>
    <summary>jack and jill are developers and are part of a project called </b>project-sapphire.</b><br/>Create a new IAM Group called <b>project-sapphire-developers</b>.</summary>

    * Use the subcommand create-group to create the group.

    1. Look up the sub-command in the [documentation](https://docs.aws.amazon.com/cli/latest/reference/iam/create-group.html)
    1. Note the single required argument - the name of the group.

    ```
    aws --endpoint http://aws:4566 iam create-group \
        --group-name project-sapphire-developers
    ```

    </details>

1.  <details>
    <summary>Add the IAM users called jack and jill, who are developers to the new IAM Group called <b>project-sapphire-developers</b>.</summary>

    * Use the subcommand `add-user-to-group` to add users into the group.

    1. Look up the sub-command in the [documentation](https://docs.aws.amazon.com/cli/latest/reference/iam/add-user-to-group.html)
    1. Note the two required options. We need to use both of them.
    1. Note that we will need to run the command twice, once for each user.

    ```
    aws --endpoint http://aws:4566 iam add-user-to-group \
        --group-name project-sapphire-developers \
        --user-name jack

    aws --endpoint http://aws:4566 iam add-user-to-group \
        --group-name project-sapphire-developers \
        --user-name jill
    ```

    </details>

1.  <details>
    <summary>What privileges are granted for jack and jill who are part of the group project-sapphire-developers?</summary>

    * Check for their permissions individually and the ones granted to the group.

    A user's permission is determined by their own directly attached polices, combined with any inherited from any groups they are a member of

    There is a subcommand for each case:

    * [list-attached-user-policies](https://docs.aws.amazon.com/cli/latest/reference/iam/list-attached-user-policies.html) for checking a user
    * [list-attached-group-policies](https://docs.aws.amazon.com/cli/latest/reference/iam/list-attached-group-policies.html) for checking a group

    ```
    aws --endpoint http://aws:4566 iam list-attached-group-policies \
        --group-name project-sapphire-developers

    aws --endpoint http://aws:4566 iam list-attached-user-policies \
        --user-name jack

    aws --endpoint http://aws:4566 iam list-attached-user-policies \
        --user-name jill
    ```

    Are there any attached policies in the output of any of the above three commands? If not, then neiter have any access.

    </details>

1.  <details>
    <summary>Both jack and jill need complete access to the EC2 service.</summary>

    * Attach the `AmazonEC2FullAccess` policy with the ARN: `arn:aws:iam::aws:policy/AmazonEC2FullAccess` to the group `project-sapphire-developers`.

    By adding a policy to the group that both members belong to, they will both inherit the permissions.

    1. Look up the sub-command in the [documentation](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-group-policy.html)
    1. Note the required arguments

    ```
    aws --endpoint http://aws:4566 iam attach-group-policy \
        --group-name project-sapphire-developers \
        --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
    ```

    </details>

