# 🔒 Política de Segurança — Observability EPO

## 📋 Versões Suportadas

| Versão | Suporte de Segurança |
|--------|----------------------|
| main   | ✅ Suportada          |
| develop| ⚠️ Somente em testes  |
| Outras | ❌ Não suportada      |

---

## 🚨 Reportando uma Vulnerabilidade

Se você encontrou uma vulnerabilidade de segurança neste repositório, **não abra uma issue pública**.

### Como reportar

1. Envie um e-mail para o time de segurança da EPO
2. Descreva detalhadamente a vulnerabilidade encontrada
3. Inclua passos para reproduzir o problema
4. Aguarde a confirmação em até **72 horas**

---

## 🛡️ Boas Práticas de Segurança

### Credenciais e Secrets
- ❌ **Nunca** commite credenciais, tokens ou senhas no repositório
- ✅ Utilize variáveis de ambiente ou cofres de secrets (ex: Vault, AWS Secrets Manager)
- ✅ Utilize o arquivo `.env.example` como referência, nunca o `.env` real

### Acesso ao Repositório
- Acesso concedido somente a membros autorizados da EPO
- Revisão de acessos realizada periodicamente
- Autenticação de dois fatores (2FA) obrigatória para todos os colaboradores

### Código
- Todo código deve passar por **Code Review** antes do merge
- Pull Requests diretos para `main` são bloqueados
- Dependências devem ser mantidas atualizadas

### Dados Sensíveis
- Logs e métricas **não devem** conter dados pessoais ou sensíveis (PII)
- Seguir as diretrizes da **LGPD** no tratamento de dados

---

## 🔄 Atualizações de Segurança

As atualizações de segurança serão aplicadas com prioridade máxima e comunicadas ao time via canais internos da EPO.

---

## 📚 Referências

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OpenTelemetry Security](https://opentelemetry.io/docs/concepts/security/)
- [Grafana Security](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/)