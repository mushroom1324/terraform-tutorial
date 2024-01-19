# Understanding The Variable Block

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

`variable block` 은 3개의 파라미터를 갖는다:
- default : (optional) 변수의 기본값을 나타낸다.
- type : (optional) 변수의 타입을 명시한다. 기본값은 `any` 이다.
  - `string`, `number`, `bool`, `any`
  - 이 외에도 `list`, `map`, `object`, `tuple` 등 여러가지가 있다.
- description : (optional) 변수가 어떤 것을 나타내는지 설명하기 위해 사용한다.


### list type

리스트는 여러 개의 요소를 나타내는 데 사용된다:

```terraform
# variables.tf

variable "prefix" {
  default = ["Mr", "Mrs", "Sir"]
  type = list
}
```

해당 변수를 사용하기 위해 **index** 로 접근한다:

```terraform
# main.tf

resource "random_pet" "my-pet" {
  prefix = var.prefix[0] # "Mr"
}
```

constraint 를 지정하기 위해 다음과 같이 설정할 수 있다:

```terraform
# variables.tf

variable "length" {
  default = [1, 2, 3]
  type = list(number)
}
```

### map type

키-값 쌍을 나타내기 위해 사용한다:

```terraform
# variables.tf

variable "file-content" {
  type = map
  default = {
    "statement1" = "We love pets!"
    "statement2" = "We love animals!"
  }
}
```

다음과 같은 방식으로 정의하며, 해당 값에 접근하기 위해 `main.tf`를 다음과 같이 구성한다:

```terraform
# main.tf

resource "local_file" "my-pet" {
  filename = "/root/pets.txt"
  content = var.file-content["statement2"] # "We love animals!"
}
```

`map` 역시 `list`와 마찬가지로 constraint 를 지정하기 위해 다음과 같이 설정할 수 있다:

```terraform
# variables.tf

variable "file-content" {
  type = map(string)
  default = {
    "statement1" = "We love pets!"
    "statement2" = "We love animals!"
  }
}
```

### set type

`set` 타입은 `list`와 유사하지만, 중복된 값을 가질 수 없다:

```terraform
# variables.tf

variable "length" {
  default = [1, 2, 3, 3]
  type = set(number)
}
```

다음과 같이 `3` 이 중복되어 있는 경우, `terraform plan` 또는 `terraform apply` 명령어 수행 시 duplicate 에러가 발생한다.

### object type

객체를 나타내기 위해 사용한다:

```terraform
# variables.tf

variable "bella" {
  type = object({
    name = string
    color = string
    age = number
    food = list(string)
    favorite_pet = bool
  })
  
  default = {
    name = "bella"
    color = "brown"
    age = 7
    food = ["fish", "chicken", "turkey"]
    favorite_pet = true
  }
}
```

다음과 같이 내부에 여러 복합적 타입을 나타낼 수 있다.

### tuple type

`tuple` 타입은 `list` 와 유사하지만 여러 변수 타입을 사용할 수 있다.

```terraform
# variables.tf

variable "length" {
  type    = tuple([string, number, bool])
  default = ["cat", 7, true]
}
```

- 타입이 맞지 않거나, 개수가 맞지 않으면 에러를 반환함에 주의한다.