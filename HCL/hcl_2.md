# HCL : HashiCorp Configuration Language

다음 예시는 테라폼의 블럭 구조 이해를 돕기 위한 코드이다:
``` terraform
resource "local_file" "pet" {
    filename = "root/pets.txt"
    content = "We love pets!"
}
```

해당 resource 에 매개변수를 추가해보자. 

`local` provider 의 `file` 에 대한 매개변수는 [테라폼 도큐먼트](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file)에서 확인할 수 있다.

``` terraform
resource "local_file" "pet" {
    filename = "root/pets.txt"
    content = "We love pets!"
    file_permission = "0700"
}
```

file_permission 을 디폴트 값인 `0777`에서 `0700` 으로 수정하였다.

이는 파일의 소유자를 제외한 다른 모든 사용자의 접근 권한을 없앤다는 의미이다.

