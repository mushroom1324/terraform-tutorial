# Resource Attribute Reference

Resource Attribute 를 사용하여 두 개의 리소스를 연결하는 방법을 알아보자.

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

현재 두 리소스 간 dependency 가 없다.

`local_file` 리소스의 content 에 `random_pet` 리소스의 id 를 추가하자.

``` terraform
# main.tf

resource "local_file" "pet" {
    filename = var.filename
    content = "My favorite pet is ${random_pet.my-pet.id}"
}

resource "random_pet" "my-pet" {
    prefix = var.prefix
    separator = var.separator
    length = var.length
}
```

- `${}` : `interpolation syntax` 으로, string 내부에서 변수를 사용할 수 있게 해준다.

이와 같은 방법으로 자원 내에 다른 자원의 attribute 를 사용할 수 있다.