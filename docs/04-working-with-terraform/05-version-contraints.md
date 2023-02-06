# Lab: Version Constraints

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

Providers are [semantically versioned](https://semver.org/) which helps you to know how much change to expect in their functionality when upgrading - i.e. is it likely to break existing configuration when upgraded.


1.  <details>
    <summary>Navigate to the directory <b>/root/terraform-projects/omega</b> where we have added a configuration file. Inspect the file and choose the correct version of the provider </summary>

    1. Navigate to the indicated directory in Explorer pane
    1. Inspect the `.tf` files. Look for a `terraform` block, where the provider and version are defined.

    </details>

1.  <details>
    <summary>Now, change to the directory <b>/root/terraform-projects/rotate</b>. We have already initialized the configuration directory using the terraform init command.<br/>Inspect the <b>rotation.tf</b> file and find out the correct version of the provider plugin that is downloaded.<br/>Choose the correct version</summary>

    1. Navigate to the indicated directory in Explorer pane
    1. Inspect the `terraform` block. There is an expression for the version, which breaks down as
        * Greater than 3.45.0 _and_
        * Not equal to 3.46.0 _and_
        * Less than 3.48.0

        Read the three constraints treating each comma as `and`. This leaves only one possible answer.

    </details>

1.  <details>
    <summary>Which one of the below is not a valid version constraint operator?</summary>

    Refer to the [documentation](https://developer.hashicorp.com/terraform/language/expressions/version-constraints#version-constraint-syntax)

    The incorrect one is `==` because it should be `=`

    </details>

1.  <details>
    <summary>We have been working on a project called nautilus under the configuration directory /root/terraform-projects/nautilus.</b>Due to a version mismatch, we don't want to download the aws provider version 3.17.0. Which version constraint can be used to achieve this?</summary>

    Since it is a specific version we don't want, then we can use the Not Equals operator which is `!=` 



    </details>

1.  <details>
    <summary>Now, navigate to the directory /root/terraform-projects/lexicorp where we have added the configuration files. Inspect the file and find out which version of providers will be download.</summary>

    1. Look at the kubernetes provider:

        * Greater than 1.12.0 _and_
        * Not equal to 1.13.1 _and_
        * Less than 1.13.3

        Terraform will choose the highest version available based on the constraints, therefore `1.13.2`

    1. Look at the helm provider. Here it is using the "pessimistic constraint" operator, which will choose the hoigest available version of the last digit in the requested version, so `~> 1.2.0` means the highest available `1.2.x`. To find `x` we need to check the [provider documentation](https://registry.terraform.io/providers/hashicorp/helm/latest/docs).

        You will note that the latest version is much higher than 1.2.4 (2.8.0 at the time of writing) so we'll need to check the version history of the provider.

        1. Near the top where it has<br/>`Providers / hashicorp / helm / Version 2.8.0`<br/>there is a drop-down arrow next to the version. Click it.
        1. In the expanded menu, click `View all versions` which will grow the menu.
        1. Scroll down to find the `1.2` versions and note the highest 1.2 version. This is the answer for the helm provider - `1.2.4`

    </details>

