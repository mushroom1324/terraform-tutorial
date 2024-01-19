# Define Input Variables

앞전에서 사용한 `main.tf` 이다:

``` terraform
# main.tf

resource "local_file" "pet" {
    filename = "root/pets.txt"
    content = "We love pets!"
}

resource "random_pet" "my-pet" {
    prefix = "Mrs"
    separator = "."
    length = 1
}
```

현재까지는 arguments 의 값들이 하드코드 되어있다.

하드코드 값의 사용은 IaC tool 의 재사용성에 좋지 않기 때문에 사용을 지양한다.

따라서 `input variables` 를 사용하는 것이 좋다.

`input variables` 를 사용하기 위해, `variables.tf` 파일을 만든다:

```terraform
# variables.tf

variable "filename" {
  default = "/root/pets.txt"
}

variable "content" {
  default = "We love pets!"
}

variable "prefix" {
  default = "Mrs"
}

variable "separator" {
  default = "."
}

variable "length" {
  default = "1"
}
```

- 값을 할당하기 위해 (optional) `default` value 를 할당한다.

이제 variables 를 사용하기 위해 `main.tf` 를 수정한다:

``` terraform
# main.tf

resource "local_file" "pet" {
    filename = var.filename
    content = var.content
}

resource "random_pet" "my-pet" {
    prefix = var.prefix
    separator = var.separator
    length = var.length
}
```

이렇게 설정하여 variables 를 수정하여 변경 사항을 적용할 수 있다.

