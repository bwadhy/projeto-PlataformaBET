# PlataformaBET - Integração CI/CD com Okta OIDC
Este documento descreve a configuração de autenticação e autorização do pipeline PlataformaBET, usado em GitHub Actions para CI/CD, integrando com Okta via OIDC.
É o acesso pro grupos/funcoes para os desenvenvolvedores


-Objetivo

Pipelines DEV/Prod usando token stateless

Escalável, seguro, audível

Autorização por grupos e scopes

-Arquitetura Conceitual

Pequeno diagrama ASCII ou texto mostrando o fluxo:

GitHub Actions → Okta (OIDC App) → Token JWT → Cloud/API/K8s


-Explicação das responsabilidades de cada componente

Passo a passo no Okta
Seção detalhada como fizemos cada passo:

Criar Authorization Server

Criar Scope deploy

Criar OIDC App plataformabet-ci-dev

Criar Policy e Rule de Client Credentials

Criar Grupo BET-Deploy-Dev

Criar Claim groups para token

Teste do token via curl

Exemplo de comando curl para teste

curl -X POST https://<OKTA_DOMAIN>/oauth2/<AUTH_SERVER_ID>/v1/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -u CLIENT_ID:CLIENT_SECRET \
  -d "grant_type=client_credentials" \
  -d "scope=deploy"


Exemplo de payload do token

{
  "sub": "plataformabet-ci-dev",
  "scp": ["deploy"],
  "groups": ["BET-Deploy-Dev"],
  "iss": "https://<OKTA_DOMAIN>/oauth2/<AUTH_SERVER_ID>",
  "exp": 1700000000
}


-Boas práticas

Token curto (5–10 min)

Stateless → validar JWT localmente

Usar grupos + scopes para autorização

Logs devem registrar workflow, commit, identidade e grupo

Próximos passos / expansão (opcional)

Criar apps para staging e prod

Criar grupos correspondentes

Adicionar claims e scopes para cada

Integrar com GitHub Actions OIDC

Autor / Data / Status

-Autor: Bruno Wadhy
Data: 10/02/2026
Status: Documentação de setup Okta CI/CD PlataformaBET
