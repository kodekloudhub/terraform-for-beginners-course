# Lab: AWS EC" and Provisioners

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).


1.  <details>
    <summary>Navigate to the directory <b>/root/terraform-projects/project-cerberus</b>. We have an empty main.tf file in this directory.<br/>Using this configuration file write a resource block to provision a simple EC2 instance with the following specifications</summary>

    Specifications:

    * Resource Name: `cerberus`
    * AMI: `ami-06178cf087598769c`, use variable named `ami`
    * region: `eu-west-2`, use variable named `region`
    * Instance Type: `m5.large`, use variable named `instance_type`

    1. Navigate to the indicated directory in the Explorer pane
    1. We have been asked to use variables. It's OK to define them in `main.tf` (terraform doesn't actually care about the filenames - it considers all files ending in `.tf` when planning in no particular order)<br/>Create the variable blocks, assigning the correct default values:

        <details>
        <summary>Reveal</summary>

        ```
        variable "ami" {
            default = "ami-06178cf087598769c"
        }

        variable "instance_type" {
            default = "m5.large"
        }

        variable "region" {
            default = "eu-west-2"
        }
        ```

        </details>

    1. Add the EC2 instance below the variables, which you should know by now is type `aws_instance`. Be sure to use the variables

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_instance" "cerberus" {
            ami           = var.ami
            instance_type = var.instance_type
        }
        ```

        </details>

    1. Deploy

        ```
        cd /root/terraform-projects/project-cerberus
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  Information only

1.  Information only

1.  <details>
    <summary>Using the public key, create a new key-pair in AWS with the following specifications</summary>

    Specifications:

    * Resource Name: cerberus-key
    * key_name: cerberus
    * Use the [file functions](https://developer.hashicorp.com/terraform/language/functions) to read the the public key `cerberus.pub`

    1. Inspect the provider documentation for the use of the [aws_key_pair](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/key_pair)
    1. Create the resource block in `main.tf`

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_key_pair" "cerberus-key" {
            key_name = "cerberus"
            public_key = file(".ssh/cerberus.pub")
        }
        ```

        </details>

    1. Deploy

    1. Deploy

        ```
        cd /root/terraform-projects/project-cerberus
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>Let us now configure the <b>cerberus</b> resource to make use of this key. Update the resource block to make use of the key called <b>cerberus</b>.</summary>

    1. Inspect the [aws_instance] documentation to know which attribute to add to the `cerberus` instance to attach a key pair. Also what value it expects. Note that we haven't been asked to make a reference expression.

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_instance" "cerberus" {
            ami           = var.ami
            instance_type = var.instance_type
            key_name      = "cerberus"
        }
        ```

        Note that adding a key pair to an existing instance is a _REPLACEMENT_ operation, that is the instance will be deleted and then recreated. You have to make these considerations when working on real production infrastructure!

        </details>


    </details>

1.  <details>
    <summary>Let us now install nginx in this EC2 instance. To do this, let's make use of the <b>user_data</b> argument.</summary>

    Although you can use heredoc syntax, it's best practice to use an external file and the `file()` function.

    Modify the resource to add the user_data

    <details>
    <summary>Reveal</summary>

    ```
    resource "aws_instance" "cerberus" {
        ami = var.ami
        instance_type = var.instance_type
        key_name  = "cerberus"
        user_data = file("./install-nginx.sh")
    }
    ```

    Don't apply it yet!

    </details>

    </details>

1.  <details>
    <summary>What will happen if we run terraform apply now?</summary>

    This question and its hint are incorrect. It should be fixed soon.

    Select the following to pass it.

    > nginx will be installed on the current server.

    In the next question it will modify (not recreate) the instance, but in reality nginx would not have been installed.

    </details>

1.  <details>
    <summary>Run terraform apply and let the EC2 instance be modified.</summary>

    ```
    terraform apply
    ```

    </details>

1.  <details>
    <summary>Where should you add a provisioner block?</summary>

    Provisioners are related to the resource they provision, therefore

    > Nested block inside the resource block

    </details>

1.  <details>
    <summary>Which of the following provisioners does not need a connection block defined?</summary>

    Provisioners that don't need to connect to remote resources don't require a connection block

    > `local-exec`

    Because it operates on the workstation where you are running terrafrom.

    </details>

1.  <details>
    <summary>What is the public IPv4 address that has been allocated to this EC2 instance?</summary>

    You can use `terraform state show` for this.

    <details>
    <summary>Reveal</summary>

    ```
    terraform state show aws_instance.cerberus
    ```

    Find `public_ip` in the output

    </details>

    </details>

1.  <details>
    <summary>Let's create an Elastic IP Address.</summary>

    Create an Elastic IP resource with the following specifications:

    * Resource Name: `eip`
    * vpc: `true`
    * instance: id of the EC2 instance created for resource cerberus (use a reference expression)
    * create a `local-exec` provisioner for the `eip` resource and use it to print the attribute called `public_dns` to a file `/root/cerberus_public_dns.txt` on the iac-server.
    <br/>
    <br/>
    * [aws_eip documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip#example-usage)
    * [local_exec documentation](https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec)

    1. Create the new resource, including its provisioner

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_eip" "eip" {
            vpc      = true
            instance = aws_instance.cerberus.id
            provisioner "local-exec" {
                command = "echo ${aws_eip.eip.public_dns} >> /root/cerberus_public_dns.txt"
            }
        }

        ```

        </details>

    1. Deploy

        ```
        terraform plan
        terraform apply
        ```

    </details>

1.  <details>
    <summary>What is the public ip address that was created for this Elastic IP?</summary>

    You can use `terraform state show` for this.

    <details>
    <summary>Reveal</summary>

    ```
    terraform state show aws_eip.eip
    ```

    Find `public_ip` in the output

    </details>

    </details>

1.  <details>
    <summary>In the current configuration, which dependency is NOT true?</summary>

    The Elastic IP resource called `eip` has a reference expression pointing to the AWS EC2 resource called cerberus. Hence the resource `eip` depends on cerberus and not the other way around.

    <details>
    <summary>Reveal</summary>

    > Resource called `cerberus` depends on the resource called `eip`

    The above is the correct answer given the logic of the statement above, but it will be marked incorrect. Logic of the question is wrong, and will be corrected. To pass the question, select the following

    > Resource called `eip` depends on the resource called `cerberus`

    </details>


    </details>

