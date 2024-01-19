# Terraform Providers

### terraform init

`terraform init` 을 실행하면, 테라폼이 provider 를 위해 필요한 plugins 를 다운로드 및 설치한다.

provider는 AWS, GCP, Azure 뿐 아니라 앞전 예시에서 다루었던 `local` 과 같은 간단한 provider 들 또한 포함한다.

테라폼은 plugin-based architecture 를 기반으로, 3800++ 개의 인프라스트럭쳐 플랫폼과 작용한다.

https://registry.terraform.io/ 에 방문하여 이를 확인할 수 있다.

`terraform init` 의 실행 예시이다:

```Shell
terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/local...
- Installing hashicorp/local v2.4.1...
- Installed hashicorp/local v2.4.1 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

- `Installing hashicorp/local v2.4.1...` : `hashicorp/local` 의 2.4.1 버전이 설치되었다.
  - `hashicorp` : organizational namespace 이다.
  - `local` : type 으로, provider 의 이름이다.

기본적으로 가장 최신 버전을 다운로드 및 설치하며, 원한다면 특정 버전으로 고정할 수 있다.
