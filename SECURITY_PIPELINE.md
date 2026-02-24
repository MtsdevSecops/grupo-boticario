# Pipeline de Segurança - VAmPI API (Projeto de Teste)

## 📋 Visão Geral

Pipeline de segurança automatizada para demonstração de análises SAST e DAST em projeto de teste.

## 🛠️ Ferramentas Implementadas

### 1. SAST - Análise Estática
**Ferramentas:** Bandit + Pylint + Safety

**Bandit:**
- Detecta vulnerabilidades de segurança em Python
- Identifica SQL Injection, hardcoded passwords, weak crypto

**Pylint:**
- Análise de qualidade de código
- Detecta bugs e code smells

**Safety:**
- Verifica vulnerabilidades em dependências
- Consulta banco de dados de CVEs

### 2. DAST - Análise Dinâmica
**Ferramenta:** OWASP ZAP

**Capacidades:**
- Testes de penetração automatizados
- Detecção de OWASP Top 10
- Análise de APIs REST em execução
- Testes de autenticação e autorização

## 🚀 Execução

### Opção 1: GitHub Actions (Automático)
```bash
git push origin main
# Ou execute manualmente em Actions > Security Scan Pipeline > Run workflow
```

### Opção 2: Execução Local

**SAST:**
```bash
# Instalar ferramentas
pip install bandit pylint safety

# Executar scans
bandit -r . -f json -o bandit-report.json
pylint **/*.py --output-format=json > pylint-report.json
safety check --json > safety-report.json
```

**DAST:**
```bash
# Iniciar aplicação
docker run -d -p 5000:5000 --name vampi erev0s/vampi:latest
curl http://localhost:5000/createdb

# Executar ZAP
docker run --network host -v $(pwd):/zap/wrk/:rw \
  -t ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py -t http://localhost:5000 -r zap-report.html

# Limpar
docker stop vampi && docker rm vampi
```

## 📊 Relatórios

**SAST:**
- `bandit-report.json` - Vulnerabilidades de segurança
- `pylint-report.json` - Qualidade de código
- `safety-report.json` - Vulnerabilidades em dependências

**DAST:**
- `zap-report.html` - Relatório visual completo
- `zap-report.json` - Dados estruturados

## 📈 Vulnerabilidades Esperadas

Como VAmPI é intencionalmente vulnerável:

**SAST detectará:**
- SQL Injection
- Hardcoded JWT secret
- Weak cryptography
- Insecure deserialization

**DAST detectará:**
- Broken authentication
- Broken authorization
- Excessive data exposure
- Lack of rate limiting

## 🔧 Configuração Opcional: SonarQube

Para análise mais avançada:

```bash
# Iniciar SonarQube local
docker run -d -p 9000:9000 sonarqube:community

# Acessar http://localhost:9000 (admin/admin)
# Gerar token e adicionar aos secrets do GitHub:
# SONAR_TOKEN e SONAR_HOST_URL
```

## 📚 Referências

- [Bandit](https://bandit.readthedocs.io/)
- [OWASP ZAP](https://www.zaproxy.org/docs/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

---

**Nota:** Projeto de teste com vulnerabilidades intencionais para fins educacionais.
