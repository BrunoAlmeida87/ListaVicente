# Configuração de Segurança do Firestore

## Situação atual
Seu banco de dados está em **Test Mode** — completamente aberto à internet. As regras expiram em **1 dia**.

## Como aplicar as regras de segurança

### Opção 1: Via Firebase Console (recomendado)

1. Abra [Firebase Console](https://console.firebase.google.com/)
2. Selecione o projeto **lista-vicente**
3. No menu esquerdo, vá para **Firestore Database**
4. Clique na aba **Regras** (no topo)
5. Copie o conteúdo do arquivo `firestore.rules` deste repositório
6. Cole no editor de regras do Firebase
7. Clique em **Publicar**

### Opção 2: Via Firebase CLI

```bash
npm install -g firebase-tools
firebase login
firebase deploy --only firestore:rules
```

## O que essas regras fazem

✅ **Permite:**
- Qualquer pessoa **ler** a lista (necessário para a app funcionar)
- Qualquer pessoa **escrever** dados válidos (adultos, crianças, categorias)
- Estrutura de dados protegida contra corrupção

❌ **Bloqueia:**
- Acesso a outros documentos/coleções
- Dados com estrutura inválida
- Listas com mais de 200 adultos ou 100 crianças

## Se precisar compartilhar com autenticação

Se quiser que apenas pessoas autenticadas (com email/senha) possam editar:

```javascript
allow write: if request.auth.uid != null
  && request.data.keys().hasAll(['adults', 'children', 'categories'])
  // ... outras validações
```

Nesse caso, sua app teria um login simples.

## Resumo

- **Não coloque dados sensíveis** nessa coleção (não há senhas armazenadas, então está ok)
- **Apenas dados de convidados** são armazenados no Firestore (status, nome, categoria, etc)
- **Acesso é público de leitura** — qualquer pessoa com o link pode ver a lista
- **Escrita é validada** — impede vandalismo e corrupção de dados

Para mais segurança, considere adicionar **autenticação anônima** ou **token de acesso** no futuro.
