# Relatório Técnico — Auditoria de Senhas em Ambiente Controlado

## 1. Objetivo
Validar técnicas básicas de força bruta e password spraying em serviços comuns (FTP, aplicações web e SMB) para entender:
- Como vetores de autenticação podem ser explorados;
- Quais evidências ficam disponíveis em logs;
- Quais medidas mitigatórias são eficazes.

## 2. Ambiente de Teste
- Host: Máquina física do autor.
- VMs:
  - **Kali Linux** (atacante, com ferramentas de auditoria instaladas).
  - **Alvo 1:** Metasploitable2 (serviços: FTP, SMB, web vulnerável).
  - **Alvo 2:** DVWA em uma VM web (formulário de login para testes).
- Rede: *Host-only* (isolada), sem acesso externo.
- Observação: snapshots das VMs foram criados antes de cada conjunto de testes para rápida restauração.

## 3. Metodologia
1. **Preparação**
   - Atualizei o Kali e verifiquei a disponibilidade das ferramentas (Medusa, Hydra, outros utilitários).
   - Organizei wordlists de exemplo (listas públicas reduzidas para testes).
2. **Enumeração**
   - Identifiquei serviços ativos (FTP, SMB, HTTP) e portas via scanner interno.
3. **Testes de Autenticação (simulados / controlados)**
   - FTP: Executados testes de força bruta utilizando uma ferramenta de linha de comando contra contas de teste nas VMs-alvo.
   - Web (DVWA): Testes de tentativa em formulário web com limites controlados.
   - SMB: Execução de password spraying contra contas de laboratório, observando bloqueios e registros.
4. **Registro de Evidências**
   - Logs das ferramentas, capturas de tela do terminal e prints de logs das VMs-alvo foram salvos em `evidencias/`.

> Nota: Para segurança e ética, os comandos reais usados na rede de laboratório foram executados apenas nas VMs isoladas. Abaixo, apresento **exemplos genéricos** de uso com placeholders em vez de instruções prontas para uso em ambientes externos.

## 4. Exemplos (genéricos, com placeholders)
- Exemplo genérico de execução de ferramenta de auditoria (substitua `<ALVO>`, `<USUARIO>`, `<WORDLIST>` e confirme que está em ambiente autorizado):
```
medusa -H <ALVO> -U <USUARIO> -P <WORDLIST> -M ftp
```
- Para testes em formulário web, recomenda-se usar módulos que suportem simulação de sessões e respeitar limites de taxa (rate limiting).  
- Para password spraying em SMB, rode variações controladas e monitore logs de bloqueio de conta.

## 5. Resultados (síntese)
- Foram identificadas contas de teste com credenciais fracas nas imagens padrão (esperado em VMs vulneráveis).  
- Observou-se que mecanismos simples de proteção (bloqueio após N tentativas, políticas de senha fortes) reduzem drasticamente a eficácia dos testes.

## 6. Recomendações de Mitigação
- **Política de senhas robusta:** comprimento mínimo, complexidade e verificação contra wordlists conhecidas.
- **Rate limiting e lockout:** bloquear ou retardar tentativas após repetidas falhas.
- **Monitoramento e alertas:** IDS/IPS e análise de logs para tentativas de autenticação suspeitas.
- **Multi-factor Authentication (MFA):** implementar onde possível.
- **Backups e resposta a incidentes:** planos e exercícios regulares.

## 7. Conclusão
O exercício demonstrou, em ambiente controlado, como técnicas de força bruta e password spraying podem comprometer sistemas sem proteções básicas. As medidas propostas formam uma linha de defesa operacional eficiente quando aplicadas em conjunto.

---

Anexos:
- `evidencias/` — capturas e logs (simulados).
- `ENTREGA.md` — texto curto para o campo de submissão.
