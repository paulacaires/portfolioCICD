# portfolioCICD
Portfólio pessoal simples em Markdown para aprender CI/CD com os serviços da AWS, com foco em fazer o deploy automático.

Link: https://paula-portfolio-ci-cd.s3.sa-east-1.amazonaws.com/index.html

# 1. Configurar GitHub localmente
1. https://docs.github.com/pt/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

2. Para configurar a chave SSH & ssh-agent no Windows:
https://docs.github.com/en/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases

3. Adicionar a chave SSH à conta do GitHub:
https://docs.github.com/en/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

# 2. Criar o bucket S3
- Desmarcar "Block all public access"

- Ativar "site estático". Na aba *Properties* do bucket, ir em "Static website hosting", dar um enable e colocar como o "Index document" o `index.html`.

- Configurar a Policy para ficar público. Em *Permissions -> Bucket Policy*:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::paula-portfolio-ci-cd/*"
    }
  ]
}
```

# 3. Criar o projeto no CodeBuild
- Project name: `portfolio-build`
- Source: GitHub
- Repository: Esse mesmo
- Branch: `main`
- OS: Linux
- Build specifications: `buildspec.yml` file
- Em "Role": Create new service role

# 4. Conexão com o GitHub e AWS

Recursos que utilizei:

📖 [GitHub App connections for GitHub and GitHub Enterprise Server](https://docs.aws.amazon.com/codebuild/latest/userguide/connections-github-app.html)

# 5. Criar o CodePipeline
