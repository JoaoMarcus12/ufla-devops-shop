# Evidências da Atividade 05 - A Stack Completa

## 1. Status da Stack
Abaixo a saída do comando `docker compose ps` demonstrando todos os serviços em estado `healthy`:

```text
# [INSIRA AQUI A SAÍDA DO COMANDO: docker compose ps]
```

## 2. Teste de Persistência de Dados
Roteiro de teste para validar o volume nomeado do PostgreSQL:

- **Passo 1: Cadastro de Produto**
  `curl -s -X POST http://localhost/api/produtos -H 'Content-Type: application/json' -d '{"nome":"Prova de persistencia","preco":9.9,"estoque":1}'`
  **Resultado:**
  ```text
  # [INSIRA AQUI A RESPOSTA DO POST]
  ```

- **Passo 2: Derrubada da Stack**
  `docker compose down`

- **Passo 3: Subida da Stack**
  `docker compose up -d --wait`

- **Passo 4: Busca do Produto**
  `curl -s 'http://localhost/api/busca?q=persistencia'`
  **Resultado:**
  ```text
  # [INSIRA AQUI A RESPOSTA DA BUSCA - DEVE RETORNAR O PRODUTO]
  ```

## 3. Imagem no GHCR
Endereço público da imagem utilizada:
`ghcr.io/joaomarcus12/ufla-shop:1.0.0`

## 4. Questão Teórica
**Por que `depends_on` sozinho não resolve a ordem de subida, e o que o `service_healthy` acrescenta?**

O `depends_on` sozinho apenas garante a ordem de **inicialização** dos containers; ele avisa ao Docker para dar o "start" no banco antes da API. No entanto, o container do banco pode ter subido, mas o processo do PostgreSQL ainda estar fazendo a inicialização interna e não aceitando conexões.

O `service_healthy` acrescenta a verificação do **estado interno** do serviço. A API só será iniciada quando o healthcheck do banco (via `pg_isready`) retornar sucesso, garantindo que o banco está realmente pronto para receber conexões e evitando erros de `Connection Refused` na subida da aplicação.
