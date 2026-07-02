---

## Stack utilizada

| Componente | Tecnologia |
|---|---|
| HSM simulado | SoftHSM2 + PKCS#11 |
| CA e certificados | OpenSSL |
| Servidor web | Apache HTTP Server |
| CA intermediária | HashiCorp Vault |
| Sistema operacional | Debian 12 |

---

## O que foi implementado

### 1. HSM (SoftHSM)
- Inicialização do token HSM com PIN
- Geração de par de chaves RSA-2048 dentro do HSM (nunca exportável)
- Listagem e verificação dos objetos no token via `pkcs11-tool`

### 2. Root CA
- Geração de CSR usando a chave protegida pelo HSM via engine PKCS#11
- Emissão do certificado Root CA auto-assinado com validade de 3650 dias
- Instalação da CA no sistema operacional

### 3. Certificado do servidor
- Geração de CSR do servidor web (`CN=lab-server`)
- Assinatura do certificado pelo Root CA via HSM (validade 365 dias)
- Verificação da cadeia: `openssl verify -CAfile ca.crt server.crt`

### 4. Apache HTTPS
- Configuração de `SSLCertificateFile`, `SSLCertificateKeyFile` e `SSLCertificateChainFile`
- Ativação do módulo SSL e reinicialização do serviço
- Validação com `openssl s_client -connect localhost:443` — resultado: `Verify return code: 0 (ok)`

### 5. HashiCorp Vault como CA Intermediária
- Deploy do Vault em modo dev
- Criação de PKI para CA intermediária (`pki_int`)
- Assinatura da CA intermediária pelo Root CA (HSM)
- Emissão automática de certificados TLS para o domínio `app.lab.local`
- Revogação de certificado e download da CRL

---

## Evidências

As capturas de tela de cada etapa estão disponíveis na pasta [`evidencias/`](./evidencias/).

| # | Etapa |
|---|---|
| 4.1 | Inicialização do token HSM |
| 4.2 | Geração do par de chaves RSA-2048 no HSM |
| 4.3 | Geração do CSR via engine PKCS#11 |
| 4.4 | Emissão do certificado Root CA |
| 4.5 | CSR do servidor web |
| 4.6 | Assinatura do certificado pelo Root CA |
| 4.7 | Instalação da CA no sistema |
| 5 | Configuração do Apache com SSL |
| 6 | Vault como CA intermediária |

---

## Conceitos demonstrados

- Proteção de chave privada com HSM (padrão PKCS#11)
- Cadeia de confiança: Root CA → Intermediate CA → Certificado de servidor
- Comunicação segura HTTPS com TLS 1.3
- Gestão do ciclo de vida de certificados (emissão, renovação, revogação)
- Certificate Revocation List (CRL)
- Automação de PKI com HashiCorp Vault

---

## Relatório técnico

O relatório completo com todos os comandos e evidências está disponível em [`docs/relatorio-pki-hsm.pdf`](./docs/relatorio-pki-hsm.pdf).