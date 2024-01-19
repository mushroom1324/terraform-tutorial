# Resource Dependencies

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

해당 파일에서 `pet` 리소스는 `my-pet` 리소스에 의존한다.

따라서 `my-pet` 리소스가 생성된 후에 `pet` 리소스가 생성된다 (삭제는 반대로 진행된다). 

``` terraform
# main.tf

resource "local_file" "pet" {
    filename = var.filename
    content = "My favorite pet is Mr.Dog"
}

resource "random_pet" "my-pet" {
    prefix = var.prefix
    separator = var.separator
    length = var.length
}
```

이렇게 `resource attribute`를 사용하지 않는 경우에도 `depends_on` argument 를 통해 리소스가 생성되는 순서를 직접 지정할 수 있다.

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

이렇게 직접 의존성을 설정하는 것을 `explicit dependency`, 즉 명시적 의존성이라고 한다.

