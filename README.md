# Automação de Testes de API - ViaCEP
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Newman](https://img.shields.io/badge/Newman-ef5b25?style=for-the-badge&logo=postman&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)

Este projeto contém a automação de testes para a API pública [ViaCEP](https://viacep.com.br/), desenvolvida para validar a integridade dos dados e a performance da consulta de endereços por CEP.

## Tecnologias Utilizadas

* **Postman**: Plataforma para desenvolvimento e execução dos testes.
* **Newman**: Runner de linha de comando para integração e execução de coleções.
* **AJV (Another JSON Schema Validator)**: Biblioteca utilizada para validação do contrato (JSON Schema).

## Cenários de Teste

Sucesso (CEP Válido)
1.  **Disponibilidade:** Confirma se o endpoint está funcional (Status Code 200).
2.  **Contrato (JSON Schema):** Garante que a estrutura da resposta contém todos os campos obrigatórios e tipos de dados corretos.
3.  **Performance:** Verifica se o tempo de resposta da API está dentro do limite aceitável de 200ms.

Erro (Bad Request - CEP Inválido)
1. **Status Code 400**: Valide que a API identifica formatos de CEP inválidos.

## Convertendo o JSON para um schema

1. **Faça a chamada para o endpoint:**
<img width="605" height="606" alt="image" src="https://github.com/user-attachments/assets/af417f73-7d70-49e1-8ee1-95b20728f469" />

2. **Transforme em JSON através de um site:** "https://www.liquid-technologies.com/online-json-to-schema-converter"
<img width="1549" height="648" alt="image" src="https://github.com/user-attachments/assets/64ba8675-464b-4ea8-90fe-dd007c00b8d6" />

   

## Scripts de Teste Implementados

```javascript
// 1. Validação de Status Code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// 2. Validação de Estrutura do Body (Schema)
pm.test('Validando estrutura do body', function () {
    const Ajv = require('ajv');
    const ajv = new Ajv();
    const jsonData = pm.response.json();
    const valid = ajv.validate(schema, jsonData);

    pm.expect(valid, JSON.stringify(ajv.errors)).to.be.true;
});

// 3. Validação de Performance
pm.test("O tempo de resposta é inferior a 200 ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});

// 4. Validar status code 400 
pm.test("Validar status code 400 ao enviar CEP inválido", function () {
    pm.response.to.have.status(400);
});
```

## Execução dos testes através do Newman (CLI)

1. **Instalação:**
```javascript
npm install -g newman

$ npm install -g newman-reporter-html
```

2. **Arquivos exportados do postman para uma pasta no meu pc**

<img width="1507" height="136" alt="image" src="https://github.com/user-attachments/assets/12236dd9-5c82-4aa4-8795-fbdb9aeca9e1" />


**Roda no terminal:** newman run "Teste-CEP.postman_collection.json" -e Environment.postman_environment.json
<img width="636" height="605" alt="image" src="https://github.com/user-attachments/assets/658e1108-2e9d-41f3-8224-29f2704b9760" />

## Geração do relatório
1. Instalar a dependência
```javascript
npm install -g newman-reporter-htmlextra
```
**Roda no terminal:** newman run "Teste-CEP.postman_collection.json" -e Environment.postman_environment.json -r htmlextra --reporter-htmlextra-displayProgressBar
<img width="856" height="861" alt="image" src="https://github.com/user-attachments/assets/8f9ead65-f39c-45e7-bd11-e782da3b2f7b" />



