# Terraform State

terraform state 는 테라폼이 관리하는 리소스의 현재 상태를 저장하는 파일이다.

이 파일을 통해 테라폼은 리소스를 관리하고, 변경사항을 추적하고, 변경사항을 적용할 때 필요한 최소한의 변경사항을 계산한다.

`terraform apply` 를 실행하면 테라폼은 `terraform.tfstate` 파일을 생성한다.

state file 은 JSON 형식으로 작성된다:

```json
{
  "version": 4,
  "terraform_version": "1.7.0",
  "serial": 10,
  "lineage": "48b3257b-807f-d32f-17c7-41fca8c275aa",
  "outputs": {
    "pet-name": {
      "value": "da.piglet",
      "type": "string"
    }
  },
  "resources": [
    {
      "mode": "managed",
      "type": "local_file",
      "name": "pet",
      "provider": "provider[\"registry.terraform.io/hashicorp/local\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "content": "My favorite pet is da.piglet",
            "content_base64": null,
            "content_base64sha256": "hE29i6OJ6nccU36gTju5XXqoBq3JVGL88CxmT5uijQs=",
            "content_base64sha512": "UQqzTp3VI66U2qKcfgxOymB+f++1PUQ8vaEgncGCBdWGgosPW/m6fVePNfDVeFRMbBE41Bhz2WM4Q0mJ2yy4UA==",
            "content_md5": "81e150200a37fd8c94e410f6d1d7c9f3",
            "content_sha1": "03e6cdd3673adb84e9c6fa92501095688848412e",
            "content_sha256": "844dbd8ba389ea771c537ea04e3bb95d7aa806adc95462fcf02c664f9ba28d0b",
            "content_sha512": "510ab34e9dd523ae94daa29c7e0c4eca607e7fefb53d443cbda1209dc18205d586828b0f5bf9ba7d578f35f0d578544c6c1138d41873d96338434989db2cb850",
            "directory_permission": "0777",
            "file_permission": "0777",
            "filename": "dwadaw",
            "id": "03e6cdd3673adb84e9c6fa92501095688848412e",
            "sensitive_content": null,
            "source": null
          },
          "sensitive_attributes": [],
          "dependencies": [
            "random_pet.my-pet"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "random_pet",
      "name": "my-pet",
      "provider": "provider[\"registry.terraform.io/hashicorp/random\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "id": "da.piglet",
            "keepers": null,
            "length": 1,
            "prefix": "da",
            "separator": "."
          },
          "sensitive_attributes": []
        }
      ]
    }
  ],
  "check_results": null
}

```

변경사항을 만든 후 다시 `terraform apply` 를 실행하면 테라폼은 기존의 `terraform.tfstate` 파일을 삭제하고 새로운 `terraform.tfstate` 파일을 생성한다.

### Terraform State and Dependency

`terraform plan`, `terraform apply` 실행 시 리소스의 dependency 를 포함한 metadata details 를 추적한다.

``` terraform
# main.tf

resource "local_file" "pet" {
    filename = var.filename
    content = "My favorite pet is Mr.Dog"
    
    depends_on = [
        random_pet.my-pet
    ]
}

resource "random_pet" "my-pet" {
    prefix = var.prefix
    separator = var.separator
    length = var.length
}
```

다음의 테라폼 파일에서, `local_file` 리소스는 `random_pet` 리소스에 의존한다.

해당 파일을 작성한 후 `terraform apply` 를 실행한 이후 리소스를 삭제하자:

``` terraform
# main.tf


```

이 상태에서 `terraform apply` 를 실행하면 테라폼은 자원을 삭제할 순서를 어떻게 추적할 수 있는가?

바로 이 때 테라폼은 `terraform.tfstate` 파일을 추적하여 dependency 를 파악한다.

### Terraform State and Performance

테라폼이 `terraform plan` 또는 `terraform apply` 를 실행할 때마다 provider 로부터 리소스의 상태를 가져오는 것은 비효율적이다.

`terraform plan --refresh=false` 명령어를 사용하면 테라폼이 명령어 실행시 state 를 refresh 하지 않고, state file 을 cache 로 사용하여 성능을 향상시킬 수 있다.

### Terrform State File

기본적으로 `terraform.tfstate` 파일은 configuration directory 에 생성된다.

하지만 여러 사용자와 함께 협업하는 경우, 본인의 local 환경에 해당 파일을 저장하는 것은 좋지 않다.

state file 을 remote storage 에 저장하여 여러 사용자가 최신의 state 를 받아보고, 동시에 테라폼 명령어를 실행하지 않도록 해야 한다.

_Amazon S3_, _Azure Blob Storage_, _Google Cloud Storage_, _HashiCorp Consul_, _HashiCorp Terraform Cloud_ 등의 remote storage 를 사용할 수 있다.

