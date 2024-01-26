# Terraform State Considerations

Terraform state 는 프로비저닝 된 리소스의 IP, DNS 등의 정보 뿐 아니라, DB의 경우 암호까지 저장된다.

팀원과 협업할 때 Terraform state 는 Github, Gitlab, Bitbucket 등의 VCS을 통해 공유하면 해당 정보가 노출될 수 있으므로 주의해야 한다.

대신, Amazon S3, Terraform Cloud 등의 remote storage 를 사용하여 state 를 저장하는 것이 권장된다.

