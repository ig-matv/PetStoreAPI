# Feature: Criar Pet (POST /pet)

Como usuário da API

Quero criar novos pets

Para que eles sejam cadastrados corretamente no sistema

---

## Background

Dado que a API Petstore está disponível

E o endpoint `/pet` está acessível

---

## Cenário: Criar pet válido

Dado que eu informo um pet com dados válidos

| Campo | Valor |
| --- | --- |
| id | 1 |
| name | Bolt |
| status | available |

Quando eu envio uma requisição POST para `/pet`

Então o status code deve ser 200 ou 201

E o response deve conter o pet criado

---

## Cenário: Criar pet sem nome

Dado que eu informo um pet sem o campo name

| Campo | Valor |
| --- | --- |
| id | 2 |
| status | available |

Quando eu envio uma requisição POST para `/pet`

Então o status code deve ser 400

E a resposta deve conter mensagem de validação de nome obrigatório

---

## Cenário: Criar pet sem ID

Dado que eu informo um pet sem o campo id

| Campo | Valor |
| --- | --- |
| name | Rex |
| status | available |

Quando eu envio uma requisição POST para `/pet`

Então o status code deve ser 400

E a resposta deve indicar que o ID é obrigatório

---

## Cenário: Criar pet com ID duplicado

Dado que já existe um pet com id "1"

E eu informo um novo pet com o mesmo id

| Campo | Valor |
| --- | --- |
| id | 1 |
| name | Max |
| status | available |

Quando eu envio uma requisição POST para `/pet`

Então o status code deve ser 409

E a resposta deve indicar conflito de ID

---

## Cenário: Criar pet com status inválido

Dado que eu informo um pet com status inválido

| Campo | Valor |
| --- | --- |
| id | 3 |
| name | Luna |
| status | running |

Quando eu envio uma requisição POST para `/pet`

Então o status code deve ser 400

E a resposta deve indicar status inválido

---

## Cenário: Criar pet com body vazio

Dado que eu envio uma requisição POST sem body

Quando eu envio a requisição para `/pet`

Então o status code deve ser 400

E a resposta deve indicar que o body é obrigatório

---

## Cenário: Criar pet com JSON malformado

Dado que eu envio um JSON inválido

Quando eu envio a requisição POST para `/pet`

Então o status code deve ser 400

E a resposta deve indicar erro de sintaxe no JSON

Exemplo:
{ id: 1 name: Bolt status: available }

# Feature: Buscar Pet (GET /pet/{id})

Como usuário da API

Quero buscar pets pelo ID

Para validar a existência e consistência dos dados retornados

---

## Background

Dado que a API Petstore está disponível

E o endpoint `/pet/{id}` está acessível

---

## Cenário: Buscar pet existente

Dado que existe um pet cadastrado com id "1"

Quando eu envio uma requisição GET para `/pet/1`

Então o status code deve ser 200

E a resposta deve conter os dados do pet

---

## Cenário: Buscar pet inexistente

Dado que não existe um pet com id "9999"

Quando eu envio uma requisição GET para `/pet/9999`

Então o status code deve ser 404

E a resposta deve indicar que o pet não foi encontrado

---

## Cenário: Buscar pet com ID negativo

Dado que eu informo um id negativo "-1"

Quando eu envio uma requisição GET para `/pet/-1`

Então o status code deve ser 400 ou 404

E a resposta deve indicar que o ID é inválido

---

## Cenário: Buscar pet com ID decimal

Dado que eu informo um id decimal "1.5"

Quando eu envio uma requisição GET para `/pet/1.5`

Então o status code deve ser 400 ou 404

E a resposta deve indicar que o formato do ID é inválido

---

## Cenário: Buscar pet com ID string

Dado que eu informo um id não numérico "abc"

Quando eu envio uma requisição GET para `/pet/abc`

Então o status code deve ser 400 ou 404

E a resposta deve indicar que o ID deve ser numérico

# Feature: Atualizar Pet (PUT /pet)

Como usuário da API

Quero atualizar informações de pets

Para manter os dados sempre consistentes no sistema

---

## Background

Dado que a API Petstore está disponível

E o endpoint `/pet` está acessível

---

## Cenário: Atualizar nome do pet

Dado que existe um pet cadastrado com id "1"

E eu informo um novo nome para o pet

| Campo | Valor |
| --- | --- |
| id | 1 |
| name | Bolt Atualizado |
| status | available |

Quando eu envio uma requisição PUT para `/pet`

Então o status code deve ser 200

E a resposta deve conter o nome atualizado

---

## Cenário: Atualizar status do pet

Dado que existe um pet cadastrado com id "1"

E eu informo um novo status para o pet

| Campo | Valor |
| --- | --- |
| id | 1 |
| name | Bolt |
| status | sold |

Quando eu envio uma requisição PUT para `/pet`

Então o status code deve ser 200

E a resposta deve conter o status atualizado

---

## Cenário: Atualizar pet inexistente

Dado que não existe um pet com id "9999"

E eu informo os dados de um pet inexistente

| Campo | Valor |
| --- | --- |
| id | 9999 |
| name | Ghost |
| status | available |

Quando eu envio uma requisição PUT para `/pet`

Então o status code deve ser 404

E a resposta deve indicar que o pet não foi encontrado

---

## Cenário: Atualizar pet com campos inválidos

Dado que existe um pet cadastrado com id "1"

E eu envio dados inválidos para atualização

| Campo | Valor |
| --- | --- |
| id | 1 |
| name | "" |
| status | invalid_status |

Quando eu envio uma requisição PUT para `/pet`

Então o status code deve ser 400

E a resposta deve indicar erro de validação nos campos enviados

# Feature: Excluir Pet (DELETE /pet/{id})

Como usuário da API

Quero excluir pets do sistema

Para manter a base de dados atualizada e limpa

---

## Background

Dado que a API Petstore está disponível

E o endpoint `/pet/{id}` está acessível

---

## Cenário: Excluir pet existente

Dado que existe um pet cadastrado com id "1"

Quando eu envio uma requisição DELETE para `/pet/1`

Então o status code deve ser 200

E o pet deve ser removido com sucesso

---

## Cenário: Excluir pet inexistente

Dado que não existe um pet com id "9999"

Quando eu envio uma requisição DELETE para `/pet/9999`

Então o status code deve ser 404

E a resposta deve indicar que o pet não foi encontrado

---

## Cenário: Excluir pet duas vezes

Dado que existe um pet cadastrado com id "1"

Quando eu envio uma requisição DELETE para `/pet/1`

Então o status code deve ser 200

E o pet deve ser removido com sucesso

Quando eu envio novamente uma requisição DELETE para `/pet/1`

Então o status code deve ser 404

E a resposta deve indicar que o pet já foi removido