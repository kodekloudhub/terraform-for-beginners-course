# Lab: DynamoDB

Help for the [VSCode editor](https://github.com/kodekloudhub/community-faq/blob/main/docs/vscode-tips.md).

1.  Navigate to the given directory in Explorer pane

1.  <details>
    <summary>We have already created a resource block for a DynamoDB table inside <b>/root/terraform-projects/DynamoDB/project-sapphire-user-data/</b>.<br/>But something is wrong with this configuration. Try running a terraform plan or validate and identify the cause of the failure.</summary>

    Know that for DynamoDB, a primary key is one of the following
    * A Hash key alone.
    * A hash key combined with a range key.

    A primary key (which can be either of the above) is required for every table.

    Know also that all dynamo attributes referred to by a `hash_key` or `range_key` terraform argument are required to have an `attribute { ... }` block in the terraform resource that describes the dynamo attribute's data type.

    Looking at the configuration given, note that the value of `hash_key` doesn't match the value of `name` in the `attribute` block, therefore the given attribute does not apply to the primary key.

    > `Attribute for primary key is missing`

    </details>

1.  Information only

1.  <details>
    <summary>Let's fix that now! Update the main.tf file so that it uses an attribute for the Primary/Hash Key.</summary>

    1. Edit the attribute block so that it matches the requirements

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_dynamodb_table" "project_sapphire_user_data" {
        name           = "userdata"
        billing_mode   = "PAY_PER_REQUEST"
        hash_key       = "UserId"

            attribute {
                name = "UserId"
                type = "N"
            }
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/DynamoDB/project-sapphire-user-data
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

1.  Navigate to the given directory in Explorer pane

1.  <details>
    <summary>What is the name of the DynamoDB table resource that is created by this configuration?</summary>

    Remember that resource name is the second property of a `resource` statement

    <details>
    <summary>Reveal</summary>

    > `project_sapphire_inventory`

    </details>

    </details>

1.  <details>
    <summary>What is the name of the DynamoDB Table that is created this configuration?</summary>

    Inspect the `name` argument in the resource block

    <details>
    <summary>Reveal</summary>

    > `inventory`

    </details>
    </details>

1.  <details>
    <summary>How many attributes are defined in this table currently?</summary>

    Count the `attribute` blocks

    </details>

1.  <details>
    <summary>What is the name and type of the Primary Key used by this table?</summary>

    * The key name is the value of the `hash_key` argument
    * The type is found by looking at the corresponding attribute

    <details>
    <summary>Reveal</summary>

    > `AssetID - Number`

    </details>


    </details>

1.  <details>
    <summary>Now, let's add an item to this table called <b>inventory</b>. Use the following specifications and update the <b>main.tf</b> file:</summary>

    * Resource Name: `upload`
    * Table = Use reference expression to the table called `inventory`
    * Hash Key = Use reference expression to use the primary key used by the table `inventory`
    * Data
        ```json
        {
            "AssetID": {"N": "1"},
            "AssetName": {"S": "printer"},
            "age": {"N": "5"},
            "Hardware": {"B": "true" }
        }
        ```

    Some advanced concepts here. We will need a couple of reference expressions into the table resource, plus we will need to use [heredoc](https://developer.hashicorp.com/terraform/language/expressions/strings#heredoc-strings) syntax to pass the JSON block of data to the new item. Note that with heredocs, they are delimited at start and end with something like `EOT`, `EOF` or `ITEM`. Doesn't matter what you use for the delimeter as long as it's made of letters, is by convention CAPITALS and is the same at start and end.

    1. We add items using [aws_dynamodb_table_item](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table_item) resource.

        <details>
        <summary>Reveal</summary>

        ```
        resource "aws_dynamodb_table_item" "upload" {
            table_name = aws_dynamodb_table.project_sapphire_inventory.name
            hash_key   = aws_dynamodb_table.project_sapphire_inventory.hash_key
            item       = <<EOF
        {
            "AssetID": {"N": "1"},
            "AssetName": {"S": "printer"},
            "age": {"N": "5"},
            "Hardware": {"B": "true" }
        }
        EOF
        }
        ```

        </details>

    1. Deploy

        ```bash
        cd /root/terraform-projects/DynamoDB/project-sapphire-inventory
        terraform init
        terraform plan
        terraform apply
        ```

    </details>

