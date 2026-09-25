# Desafio DevSecOps — Gerenciador de Tarefas

## Sobre o Projeto
Este repositório faz parte do desafio prático do módulo de DevSecOps da ADA Tech.
Você receberá este projeto com vulnerabilidades propositais e uma pipeline incompleta.
Seu objetivo é **implementar a pipeline de segurança** e **corrigir as vulnerabilidades**.

## Estado atual
A pipeline está **incompleta**. Os steps de segurança precisam ser implementados por você.

## Sua missão
1. Implementar os steps de segurança no `pipeline.yml`
2. Fazer a pipeline **quebrar** ao detectar os problemas
3. Corrigir as vulnerabilidades encontradas
4. Fazer a pipeline **passar** com tudo verde ✅
5. Documentar o funcionamento da pipeline neste README

## O que implementar
- [ ] Secrets Scanning com **Gitleaks**
- [ ] SAST com **Semgrep**
- [ ] SCA com **Grype**
- [ ] Deploy com **GitHub Pages**

## Como a pipeline funciona

A pipeline funciona verificando a segurança do projeto automaticamente antes de permitir o deploy em produção. Cada Step está configurado para encontrar vulnerabilidades desde o início do processo de desenvolvimento.

### 1. Secrets Scanning — Gitleaks

Ao executar o Gitleaks são encontrados segredos expostos no código, como chaves de API, tokens e senhas.

Essa etapa é importante porque um segredo exposto no código pode permitir acesso indevido a sistemas ou serviços. Se um segredo for encontrado, a pipeline falha e o deploy não deve continuar.

Nesta etapa foram identificados:

Vazamento de Token de Acesso / Chave de API 

Permite que invasores acessem repositórios privados, modifiquem código ou roubem propriedade intelectual, dependendo das permissões associadas ao token.

Exposição de Senha de Banco de Dados

Dá acesso direto ao banco de dados da empresa, permitindo a exfiltração de dados sensíveis (LGPD/GDPR), adulteração de informações.


### 2. SAST — Semgrep

O SAST é importante porque permite encontrar problemas de segurança no código antes que a aplicação chegue à produção.
Ao executar o Semgrep é realizada uma análise estática do código-fonte para identificar padrões que podem representar vulnerabilidades de segurança.

Nesta etapa foram identificados:

Cross-Site Scripting - Risco de XSS
Mapeamento: OWASP Top 10: A03:2021-Injection
Risco de XSS (Cross-Site Scripting) causado pelo uso de innerHTML com um valor que vem diretamente do usuário. Isso permite o roubo de cookies de sessão, tokens de autenticação e ações maliciosas em nome do usuário afetado. 


Injeção de Código (Code Injection)
Mapeamento: OWASP Top 10: A03:2021-Injection
O uso de eval() faz o JavaScript interpretar uma string como código. Quando essa string envolve uma entrada que pode vir do usuário, existe risco de injeção de código.

### 3. SCA — Grype

O Grype verifica dependências, pacotes e componentes utilizados pelo projeto e procura vulnerabilidades conhecidas, identificadas por CVEs.
Essa etapa é importante porque uma aplicação pode ter código próprio seguro, mas ainda estar vulnerável por utilizar bibliotecas de terceiros desatualizadas.

Nesta etapa o Grype apresentou erro na implementação.
Apesar de eu ter corrigido o erro da instalação verificamos que a execução não havia encontrado nenhuma vulnerabilidade.
Para a correção executamos o comando npm install, criamos a pasta node modules e o arquivo package-lock.json que foi adicionado ao GitHub. 
Após essa etapa executamos novamente o Grype e encontramos vulnerabilidades nas versões antigas das dependências. 
As dependências foram atualizadas, o scan foi executado novamente e o gate de segurança passou.

A execução identificou essas 3 dependências abaixo e outras dependências indiretas.
lodash: 4.18.1 versao atualizda
express: 4.22.3 versao atualizda
axios: 1.20.0 versao atualizda

### 4. Deploy — GitHub Pages

O deploy é executado somente depois que os gates de segurança anteriores passam.

Quando uma vulnerabilidade é encontrada, ocorre o bloqueio da pipeline, a execução falha e o deploy não pode prosseguir.

Dessa forma, a pipeline funciona como uma barreira de segurança entre o código desenvolvido e o ambiente de produção.

### Fluxo da pipeline

O fluxo de segurança pode ser resumido da seguinte forma:

Código → Secrets Scanning → SAST → SCA → Deploy

Os três gates de segurança precisam passar para que o deploy seja realizado.

### URL de Produção

A aplicação está disponível publicamente no GitHub Pages:

https://fabianaopalmeida.github.io/projeto-devsecop-desafio/

A aplicação publicada é o **Gerenciador de Tarefas** e representa a versão de produção do projeto.

