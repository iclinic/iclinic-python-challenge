![iClinic logo](https://d1ydp7gtfj5fb9.cloudfront.net/static/img/views/home_v2/header/logo.png?1525283729)

# Desafio

A missão da iClinic é descomplicar a saúde no Brasil levando mais gestão a clínicas e consultórios através da tecnologia e, com isso, possibilitar que médicos e outros profissionais promovam mais saúde aos seus pacientes.

Seu desafio será desenvolver um serviço de prescrição médica e, como parte dele, veremos como você estrutura as camadas de aplicação, chamadas externas, variáveis de ambiente, cache, testes unitários, logs e documentação.

### Solução
Implementar uma API REST, contendo o endpoint `[POST] /prescriptions`, para inserir novas prescrições.

 - O serviço de prescrição deverá persistir no banco de dados somente os atributos recebidos no request;
 - Os [serviços dependentes](#Servi%C3%A7os-dependentes) deverão ser consultados para compor os dados a serem enviados ao serviço de métricas;
 - Se o serviço de clínicas não responder, o request deverá seguir normalmente, pois o nome da clínica é o único atributo não obrigatório do serviço de métricas;
 - Os dados deverão ser integrados com o serviço de métricas, caso isso não ocorra (por qualquer motivo) deverá ser feito `rollback` e falhar o request;
 - A API REST deverá retornar um erro quando exceder o timeout ou a quantidade de tentativas de algum serviço dependente;

Considere as informações abaixo para desenvolver o teste. Se tiver algum tipo de erro não mapeado fique à vontade para adicionar `:)`

*Request*
```bash
curl -X POST \
  http://localhost:5000/prescriptions \
  -H 'Content-Type: application/json' \
  -d '{
  "clinic": {
    "id": 1
  },
  "physician": {
    "id": 1
  },
  "patient": {
    "id": 1
  },
  "text": "Dipirona 1x ao dia"
}'
```

*Response.body*
```json
{
  "data": {
    "id": 1,
    "clinic": {
      "id": 1
    },
    "physician": {
      "id": 1
    },
    "patient": {
      "id": 1
    },
    "text": "Dipirona 1x ao dia",
    "metric": {
      "id": 1
    }
  }
}
```

*Response.body (error)*
```json
{
  "error": {
    "message": "patient not found",
    "code": "03"
  }
}
```

### Tipos de erros sugeridos
| code | message                          |
|------|----------------------------------|
| 01   | malformed request                |
| 02   | physician not found              |
| 03   | patient not found                |
| 04   | metrics service not available    |
| 05   | physicians service not available |
| 06   | patients service not available   |


### Serviços dependentes
| host                                                      | method | path            | authorization header                                                                                                                                                                             | timeout | retry | cache ttl |
|-----------------------------------------------------------|--------|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|-------|-----------|
| https://mock-api-challenge.dev.iclinic.com.br          | GET    | /physicians/:id/ | Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJzZXJ2aWNlIjoicGh5c2ljaWFucyJ9.Ei58MtFFGBK4uzpxwnzLxG0Ljdd-NQKVcOXIS4UYJtA | 4s      | 2     | 48hrs     |
| https://mock-api-challenge.dev.iclinic.com.br                | GET    | /clinics/:id/    | Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJzZXJ2aWNlIjoiY2xpbmljcyJ9.r3w8KS4LfkKqZhOUK8YnIdLhVGJEqnReSClLCMBIJRQ     | 5s      | 3     | 72hrs     |
| https://mock-api-challenge.dev.iclinic.com.br            | GET    | /patients/:id/   | Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyLCJzZXJ2aWNlIjoicGF0aWVudHMifQ.Pr6Z58GzNRtjX8Y09hEBzl7dluxsGiaxGlfzdaphzVU   | 3s      | 2     | 12hrs     |
| https://mock-api-challenge.dev.iclinic.com.br | POST   | /metrics/    | Bearer SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c                                                                                                                                               | 6s      | 5     |           |

*Metrics Request.body*
```bash
curl -X POST \
  https://mock-api-challenge.dev.iclinic.com.br/metrics/ \
  -H 'Authorization: Bearer SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c' \
  -H 'Content-Type: application/json' \
  -d '{
  "clinic_id": 1,
  "clinic_name": "Clínica A",
  "physician_id": 1,
  "physician_name": "José",
  "physician_crm": "SP293893",
  "patient_id": 1,
  "patient_name": "Rodrigo",
  "patient_email": "rodrigo@gmail.com",
  "patient_phone": "(16)998765625",
  "prescription_id": 1
}'
```

*Metrics Response.body*
```json
{
    "id": "1",
    "clinic_id": 1,
    "clinic_name": "Clinica A",
    "physician_id": 1,
    "physician_name": "José",
    "physician_crm": "SP293893",
    "patient_id": 1,
    "patient_name": "Rodrigo",
    "patient_email": "rodrigo@gmail.com",
    "patient_phone": "(16)998765625",
    "prescription_id": 1
}
```


### Testando a sua API

Para auxiliar você a testar a aderência da sua aplicação ao que foi especificado, disponibilizamos uma [ferramenta baseada em ScanAPI](https://python-challenge-scanapi.dev.iclinic.com.br/) que faz algumas chamadas de teste à sua API e diz se está tudo conforme esperado ou se há algum problema.
Para que ela funcione, você precisará que a aplicação esteja acessível publicamente, bastando inserir a URL da sua aplicação. Isso pode ser feito através de um serviço gratuito como o [Heroku](https://www.heroku.com/), por exemplo.

É apenas uma ferramenta auxiliar: o uso não é obrigatório, nem será levado em consideração para a avaliação. Porém, é recomendável, e as chamadas podem ser feitas quantas vezes você desejar.


### O que esperamos
 - Que o desafio seja feito em Python 3+;
 - Passo-a-passo de como rodar sua aplicação;
 - Clareza no código;
 - Que considere as colunas de `timeout`, `retry` e `cache ttl` nas chamadas dos serviços dependentes;
 - Testes unitários com cobertura `>= 80%`;
 - Princípios SOLID;
 - [12Factor](https://12factor.net/);
 - Gerenciamento de dependências;
 - Commits semânticos;
 - Tratamento de erros e timeouts no acesso aos serviços externos;

### Não obrigatório, mas recomendado
 - CI (Travis, Gitlab, Circle CI, Github Actions, ...) - extremamente recomendado na vaga para sênior e pleno
 - Executar a aplicação e base de dados em Docker
 - Efetuar o deploy da aplicação em algum Cloud Provider (Heroku, AWS, Google Cloud, Azure, Digital Ocean, etc) 

### Observações
 - Disponibilizar a URL do Deploy da Aplicação no Readme.md
 - Disponibilizar a documentação da API desenvolvida
___
Boa Sorte,
Equipe iClinic.
