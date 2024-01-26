# HCL : HashiCorp Configuration Language

### change configuration

다음 예시는 테라폼의 블럭 구조 이해를 돕기 위한 코드이다:
``` terraform
resource "local_file" "pet" {
    filename = "root/pets.txt"
    content = "We love pets!"
}
```

해당 resource 를 가지고 `terraform apply`까지 마친 후, 매개변수를 추가해보자. 

> [!TIP]
> `local` provider 의 `file` 에 대한 매개변수는 [테라폼 도큐먼트](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file)에서 확인할 수 있다.

``` terraform
resource "local_file" "pet" {
    filename = "root/pets.txt"
    content = "We love pets!"
    file_permission = "0700"
}
```

file_permission 을 디폴트 값인 `0777`에서 `0700` 으로 수정하였다.

이는 파일의 소유자를 제외한 다른 모든 사용자의 접근 권한을 없앤다는 의미이다.

수정 후 터미널에 `terraform plan` 을 수행하면 다음과 같은 output을 확인할 수 있다:

```Shell
terraform plan
local_file.pet: Refreshing state... [id=cba595b7d9f94ba1107a46f3f731912d95fb3d2c]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
-/+ destroy and then create replacement

Terraform will perform the following actions:

  # local_file.pet must be replaced
-/+ resource "local_file" "pet" {
      ~ content_base64sha256 = "zUA5Ip/IeKlmTQIptlp90JdMGAd8YLStDXhpGq0Bp0c=" -> (known after apply)
      ~ content_base64sha512 = "tduqTz5S8Wa3O9Ab5+g0GcGL6kMjMh61vjFcMm5KkOO5TgViAC/kBOdvYHl9qky2K99+u80z0CfCs2ExsHbjGg==" -> (known after apply)
      ~ content_md5          = "f510a471c5dc0bcd4759ad9dc81a516f" -> (known after apply)
      ~ content_sha1         = "cba595b7d9f94ba1107a46f3f731912d95fb3d2c" -> (known after apply)
      ~ content_sha256       = "cd4039229fc878a9664d0229b65a7dd0974c18077c60b4ad0d78691aad01a747" -> (known after apply)
      ~ content_sha512       = "b5dbaa4f3e52f166b73bd01be7e83419c18bea4323321eb5be315c326e4a90e3b94e0562002fe404e76f60797daa4cb62bdf7ebbcd33d027c2b36131b076e31a" -> (known after apply)
      ~ file_permission      = "0777" -> "0700" # forces replacement
      ~ id                   = "cba595b7d9f94ba1107a46f3f731912d95fb3d2c" -> (known after apply)
        # (3 unchanged attributes hidden)
    }

Plan: 1 to add, 0 to change, 1 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run
"terraform apply" now.
```

- `# local_file.pet must be replaced` : pet 파일이 replaced 되어야 한다고 말해주고 있다.
- `-/+ resource "local_file" "pet" {` : `-/+` 는 파일이 삭제된 후, 다시 생성될 것이라는 의미를 가진다.
- 이러한 **immutable infrastructure** 의 경우 테라폼이 기존 리소스를 삭제한 후 다시 생성한다.

### terraform destroy

해당 명령어를 수행하여 생성한 자원을 삭제할 수 있다:
```Shell
terraform destroy
```

명령어 실행 시, 재차 확인 과정을 거치는데, `yes` 를 입력하자.

```Shell
terraform destroy
local_file.pet: Refreshing state... [id=cba595b7d9f94ba1107a46f3f731912d95fb3d2c]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  - destroy

Terraform will perform the following actions:

  # local_file.pet will be destroyed
  - resource "local_file" "pet" {
      - content              = "We love pets!" -> null
      - content_base64sha256 = "zUA5Ip/IeKlmTQIptlp90JdMGAd8YLStDXhpGq0Bp0c=" -> null
      - content_base64sha512 = "tduqTz5S8Wa3O9Ab5+g0GcGL6kMjMh61vjFcMm5KkOO5TgViAC/kBOdvYHl9qky2K99+u80z0CfCs2ExsHbjGg==" -> null
      - content_md5          = "f510a471c5dc0bcd4759ad9dc81a516f" -> null
      - content_sha1         = "cba595b7d9f94ba1107a46f3f731912d95fb3d2c" -> null
      - content_sha256       = "cd4039229fc878a9664d0229b65a7dd0974c18077c60b4ad0d78691aad01a747" -> null
      - content_sha512       = "b5dbaa4f3e52f166b73bd01be7e83419c18bea4323321eb5be315c326e4a90e3b94e0562002fe404e76f60797daa4cb62bdf7ebbcd33d027c2b36131b076e31a" -> null
      - directory_permission = "0777" -> null
      - file_permission      = "0700" -> null
      - filename             = "root/pets.txt" -> null
      - id                   = "cba595b7d9f94ba1107a46f3f731912d95fb3d2c" -> null
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

local_file.pet: Destroying... [id=cba595b7d9f94ba1107a46f3f731912d95fb3d2c]
local_file.pet: Destruction complete after 0s

Destroy complete! Resources: 1 destroyed.
```

- 변경사항 옆에 하이픈 (`-`) 이 붙어있는데, 이는 자원의 삭제를 의미한다.

