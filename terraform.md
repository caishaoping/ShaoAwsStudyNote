

1. Some of supported secret stores and data source combos: 
AWS Secrets Manager and the aws_secretsmanager_secret_version data source 
AWS Systems Manager Parameter Store and the aws_ssm_parameter data source
AWS Key Management Service (AWS KMS) and the aws_kms_secrets data source
HashiCorp Vault and the vault_generic_secret data source

2. Web server terraform can read data of a database terraform via the database's 
terraform_remote_state, as following: 

data.terraform_remote_state.<NAME>.outputs.<ATTRIBUTE>

data "terraform_remote_state" "db" {
  backend = "s3"  
  
  config = {
      bucket = "(YOUR_BUCKET_NAME)"    
      key    = "stage/data-stores/mysql/terraform.tfstate"    
      region = "us-east-2"  
  }
}

3. Instead of define User Data script via inline, you can use template_file 
data source to do the same, or file function if there is no need to read params
 from terraform state.

data "template_file" "user_data" {
  template = file("user-data.sh")  
  vars = {    
  server_port = var.server_port    
  db_address  = data.terraform_remote_state.db.outputs.address    
  db_port     = data.terraform_remote_state.db.outputs.port  
  }
}



