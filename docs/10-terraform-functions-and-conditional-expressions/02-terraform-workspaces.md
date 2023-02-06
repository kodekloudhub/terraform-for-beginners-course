# Lab: Terraform Workspaces

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  <details>
    <summary>When we start off and create a configuration in terraform, what is the workspace that is created, to begin with?</summary>

    <details>
    <summary>Reveal<summary>

    > `default`

    </details>

    </details>

1.  <details>
    <summary>Navigate to the configuration directory <b>/root/terraform-projects/project-sapphire</b>. We have a few configuration files already created here. How may workspaces are created for this configuration currently?</summary>

    Run: `terraform workspace list` and count the number of workspaces.

    <details>
    <summary>Reveal<summary>

    ```bash
    cd /root/terraform-projects/project-sapphire
    teraaform workspace list
    ```

    > `1` - Only `default` workspace exists so far.

    </details>

    </details>

1.  <details>
    <summary>Create three new workspaces called <b>us-payroll</b>, <b>uk-payroll</b> and <b>india-payroll</b>.</summary>

    Run: `terraform workspace new` to create workspaces.

    <details>
    <summary>Reveal<summary>

    ```bash
    cd /root/terraform-projects/project-sapphire
    terraform workspace new us-payroll
    terraform workspace new uk-payroll
    terraform workspace new india-payroll
    ```

    </details>

    </details>

1.  <details>
    <summary>Now, switch to the workspace called <b>us-payroll</b>.</summary>

    Run: `terraform workspace select` to change workspaces.

    <details>
    <summary>Reveal<summary>

    ```
    terraform workspace select us-payroll
    ```

    </details>

    </details>

1.  <details>
    <summary>Where would the state file for the workspace called <b>india-payroll</b> be stored??</summary>

    Choose the correct path relative to the current configuration directory (`/root/terraform-projects/project-sapphire`)

    <details>
    <summary>Reveal<summary>

    > `terraform.tfstate.d/india-payroll`

    When using multiple workspaces, state files are stored in a directory that is the name of the workspace, beneath directory `terraform.state.d`

    </details>

    </details>

1.  Information only.

1.  <details>
    <summary>What type of variable is region?</summary>

    Inspect `variables.tf`

    <details>
    <summary>Reveal<summary>

    > `map`

    </details>

    </details>

1.  <details>
    <summary>What is the default value of the key called <b>india-payroll</b> for the variable <b>region</b>?</summary>

    Inspect `variables.tf`

    <details>
    <summary>Reveal<summary>

    > `ap-south-1`

    </details>

    </details>

1.  <details>
    <summary>What is the default value of the key called <b>india-payroll</b> for the variable <b>ami</b>?</summary>

    Inspect `variables.tf`

    <details>
    <summary>Reveal<summary>

    > `ami-55140119877avm`

    </details>

    </details>

1.  <details>
    <summary>Now, update the <b>main.tf</b> of the root module to call the child module located at /<b>root/terraform-projects/modules/payroll-app</b>. </summary>

    Adhere to the following specifications:

    * module name: `payroll_app`<br/> This module expects two mandatory arguments:
        1. `app_region` - use the values from variable called `region`
        1. `ami` - use the values from the variable called `ami`

    The values for these two arguments should be selected based on the workspace you are on.

    For example, if on `us-payroll` workspace, the `app_region` should be `us-east-1` and the `ami` `ami-24e140119877avm` OR for `uk-payroll`, the `app_region` should be `eu-west-2` and the `ami` `ami-35e140119877avm` etc.

    Know that in code we can determine the current workspace using the expression `terraform.workspace`

    1. Examine the indicated README file to learn the module's arguments. There are three arguments. One has been supplied, and the other two we must determine the values for. You will need to select the appropriate values based on the current value of `terraform.workspace`. Since our input variables are maps, we must do a [lookup](https://developer.hashicorp.com/terraform/language/functions/lookup) on them.

    1. Create the module block

        <details>
        <summary>Reveal<summary>

        ```
        module "payroll_app" {
            source = "/root/terraform-projects/modules/payroll-app"
            app_region = lookup(var.region, terraform.workspace)
            ami        = lookup(var.ami, terraform.workspace)
        }
        ```

        Note that since we are not using the default value argument of `lookup`, the following is also correct

        ```
        module "payroll_app" {
            source = "/root/terraform-projects/modules/payroll-app"
            app_region = var.region[terraform.workspace]
            ami        = var.ami[terraform.workspace]
        }
        ```

        </details>

    1. Init the configuration

        ```bash
        cd /root/terraform-projects/project-sapphire
        terraform init
        ```

    </details>

1.  <details>
    <summary>Now, using the same configuration, create the resources on all three workspaces that you created earlier!</summary>

    For this, we must select each workspace in turn and apply the configuration.

    <details>
    <summary>Reveal<summary>

    ```
    terraform workspace select us-payroll
    terraform apply -auto-approve
    terraform workspace select uk-payroll
    terraform apply -auto-approve
    terraform workspace select india-payroll
    terraform apply -auto-approve
    ```

    </details>


    </details>

