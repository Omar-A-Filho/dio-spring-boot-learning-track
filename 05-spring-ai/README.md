# DIO Spring Boot — Projeto Final 05: Spring AI

## 1. Introdução

Este módulo integra recursos de Inteligência Artificial a uma API de gerenciamento de transações financeiras, mantendo a arquitetura em camadas utilizada ao longo da trilha de estudos DIO Spring Boot.

O objetivo é explorar a integração de recursos de IA em uma aplicação Java, preservando a separação entre domínio, casos de uso e infraestrutura.

## 2. Contexto do projeto

A aplicação permite processar comandos de voz para registrar transações financeiras.

### Fluxo de processamento de áudio

1. O cliente envia um arquivo de áudio.
2. O serviço de transcrição converte o áudio em texto utilizando Whisper.
3. O modelo de linguagem interpreta a solicitação.
4. A aplicação executa o caso de uso correspondente.
5. Quando solicitado um registro, a transação é persistida no banco de dados.
6. A API retorna uma resposta em texto.

## 3. Arquitetura do projeto

O projeto mantém a organização em camadas:

- `src/main/java/dio/budgeting/domain`
  - Modelo de domínio e contratos dos repositórios.
- `src/main/java/dio/budgeting/application`
  - Casos de uso responsáveis pelas operações da aplicação.
- `src/main/java/dio/budgeting/infrastructure`
  - Controladores HTTP, persistência e integrações externas.

## 4. Tecnologias e recursos utilizados

- Java
- Spring Boot
- Spring AI
- Groq API
- Whisper (`whisper-large-v3-turbo`)
- Modelo de linguagem `openai/gpt-oss-20b`
- H2 Database
- Gradle
- JUnit 5

### 4.1. Adaptação do provedor de IA

Durante o desenvolvimento, foi realizada a substituição da integração originalmente prevista com a OpenAI pela Groq, utilizando sua API compatível com o padrão OpenAI.

Essa adaptação permitiu manter a proposta de integração com Spring AI, utilizando os serviços de modelo de linguagem e transcrição de áudio disponibilizados pela Groq.

A alteração foi realizada como uma decisão de implementação durante o processo de aprendizado e experimentação com as tecnologias utilizadas no projeto.

### 4.2. Speech-to-Text

Utiliza o recurso de transcrição de áudio do Spring AI, integrado ao serviço Whisper disponibilizado pela Groq.

### 4.3. Tool Calling

O `ChatClient` permite disponibilizar operações da aplicação ao modelo por meio de ferramentas associadas aos casos de uso.

### 4.4. Adaptação da resposta ao usuário

Na implementação realizada, a resposta final da aplicação é apresentada em texto, em vez de ser convertida em áudio (Text-to-Speech), como previsto na proposta original.

Essa adaptação simplificou o fluxo de comunicação com o usuário, mantendo o processamento do áudio de entrada, a interpretação da solicitação pela IA e a execução das operações da aplicação.

## 5. Banco de dados

O projeto utiliza H2 em memória para armazenar as transações.

**Observação:** os dados são mantidos enquanto a aplicação está em execução. Ao encerrar ou reiniciar a aplicação, o banco em memória é recriado e os registros anteriores são perdidos.

## 6. Configuração da API Groq

É necessário possuir uma chave de API da Groq.

Configure a variável de ambiente antes de iniciar a aplicação:

```bash
export GROQ_API_KEY="sua_chave_aqui"
```

A chave é utilizada pela configuração do Spring AI em `application.properties`.

Não compartilhe ou publique sua chave de API no GitHub.

## 7. Como executar

Na pasta do projeto, execute os testes:

```bash
./gradlew test
```

Para iniciar a aplicação:

```bash
./gradlew bootRun
```

## 8. Endpoints principais

| Método | Endpoint | Finalidade |
|---|---|---|
| POST | `/transactions` | Registrar uma transação |
| GET | `/transactions/{category}` | Consultar transações por categoria |
| POST | `/transactions/ai` | Enviar áudio para processamento com IA |

O endpoint `/transactions/ai` recebe um arquivo de áudio por meio de uma requisição `multipart/form-data`.

## 9. Validação e tratamento de erros

O projeto inclui:

- Validação para impedir o registro de transações com valor menor ou igual a zero.
- Tratamento global de exceções com `@RestControllerAdvice`.
- Retorno HTTP 400 para valores inválidos.
- Testes automatizados executados com Gradle.

## 10. Documentação oficial

- [Spring AI Reference](https://docs.spring.io/spring-ai/reference/index.html)
- [ChatModel API](https://docs.spring.io/spring-ai/reference/api/chatmodel.html)
- [ChatClient API](https://docs.spring.io/spring-ai/reference/api/chatclient.html)
- [Tools API](https://docs.spring.io/spring-ai/reference/api/tools.html)
- [Audio Transcriptions API](https://docs.spring.io/spring-ai/reference/api/audio/transcriptions.html)

## 11. Contexto de desenvolvimento e aprendizado

Este projeto foi desenvolvido durante meu primeiro semestre do curso de Análise e Desenvolvimento de Sistemas (ADS), em uma fase de transição dos fundamentos da linguagem Java para os primeiros contatos com Spring Boot e Spring AI.

Por estar no início dessa jornada, a maior parte das implementações e etapas de maior complexidade contou com o apoio de ferramentas de Inteligência Artificial, utilizadas como recurso de orientação, explicação e auxílio técnico durante o desenvolvimento.

Meu conhecimento atual sobre algumas dessas implementações ainda é parcial. Determinados conceitos, estruturas e decisões técnicas estão além do conteúdo que já consolidei em Java e fazem parte dos próximos estágios da minha formação.

Assim, este projeto representa principalmente uma experiência prática de exploração tecnológica e aprendizado orientado, permitindo meu primeiro contato com integração de serviços de IA, processamento de áudio, APIs e recursos do ecossistema Spring.

O objetivo não é apresentar domínio avançado dessas tecnologias, mas registrar uma etapa real do meu desenvolvimento como estudante, identificando conhecimentos adquiridos e pontos que ainda precisam ser aprofundados.

## 12. Considerações finais

Projeto educacional desenvolvido como parte da trilha DIO Spring Boot, com foco na experimentação de recursos de Inteligência Artificial, processamento de áudio e integração com uma aplicação Java organizada em camadas.

A implementação representa uma etapa inicial de aprendizado e serve como referência para estudos e aprofundamentos futuros no ecossistema Spring.