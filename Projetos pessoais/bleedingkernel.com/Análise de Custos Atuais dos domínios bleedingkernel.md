# Análise de Custos Atuais dos domínios bleedingkernel

| Domínio             | Plataforma  | Preço Atual (BRL) | Preço Atual (USD) | Notas                    |
| ------------------- | ----------- | ----------------- | ----------------- | ------------------------ |
| bleedingkernel.com  | Squarespace | R$ 67,20          | ~$12,13           | **Preço promocional**    |
| bleedingkernel.com  | Cloudflare  | R$ 57,80          | $10,44            | Cotação: USD 1 = R$ 5,54 |
| bleedingkernel.cc   | Cloudflare  | R$ 44,32          | $8,00             | Preço fixo em USD        |
| nicolasoliveira.dev | Squarespace | R$ 50,00          | ~$9,03            | Preço estável até agora  |

### ⚠️ Riscos Específicos de Cada Plataforma

**Squarespace:**

- **Vantagem:** Preços em BRL (proteção cambial)
- **Risco real:** 
  - O preço promocional (R$67,20) provavelmente é **apenas para o primeiro ano**
  - Preço de renovação deve ser o valor cheio (R$96 ≈ $17,33 USD)
  - Histórico mostra que seu domínio .dev mantém preço, mas **não há garantia formal**

**Cloudflare:**

- **Vantagem:** 
  - Preços de renovação **fixos em USD** (sem aumentos surpresa)
  - Transparência: Cobram apenas custo do registry + $0
- **Risco:** 
  - Exposição à variação cambial
  - Se dólar subir para R$6,50: 
    - `.com` passaria para R$67,86 
    - `.cc` para R$52,00

### 📌 Minha Recomendação Estratégica

**Opção 1 (Mais Econômica a Longo Prazo):**  
🔥 **Compre ambos na Cloudflare**  

```diff
+ bleedingkernel.com ($10.44 USD = R$57,80 hoje)
+ bleedingkernel.cc ($8.00 USD = R$44,32 hoje)
= Total: R$102,12 (contra R$111,52 da Squarespace + Cloudflare)
```

**Por quê?**  

- Preços de renovação são **fixos em USD** (enquanto Squarespace pode ter aumento em BRL)
- Economia de **~15%** mesmo com dólar atual
- Cenário pessimista (USD R$6,50):  
  - Renewal: R$67,86 (.com) + R$52,00 (.cc) = **R$119,86**  
  - Ainda assim mais barato que renovação da Squarespace (R$96 só pro .com!)

**Opção 2 (Conservadora):**  
⚠️ **Squarespace APENAS se confirmar preço vitalício**  

- Pergunte ao suporte: *"O valor de R$67,20 é vitalício ou só primeiro ano?"*  
- Se for vitalício: compre `.com` na Squarespace e `.cc` na Cloudflare  
- Se não: volte para Opção 1

### 🔄 Como Mitigar Riscos Cambiais na Cloudflare

1. **Tranque a cotação:**  
   - Compre dólares agora via Wise/Nomad e deixe saldo para 2-3 anos de renew
2. **Renove por múltiplos anos** (se possível)
3. **Monitore alertas de câmbio** com apps como Economatica

### 💎 Fatores Decisivos

1. **Seu domínio .dev:**  
   
   - A estabilidade de preço provavelmente é **exceção**, não regra  
   - Registradores geralmente aumentam preços após primeiro ano (especialmente em promoções)

2. **Vantagem técnica da Cloudflare:**  
   
   - DNS mais rápido + segurança integrada + proxy gratuito  
   - Essencial para servidor doméstico (protege seu IP real)

3. **Questão CPF:**  
   
   - Ambos `.com` e `.cc` na Cloudflare **não expõem CPF** (WHOIS privacy nativo)

Excelente estratégia! Comprar **ambos os domínios** (`bleedingkernel.com` + `bleedingkernel.cc`) é uma decisão inteligente, e explico por quê:

---

### ✅ **Vantagens dessa abordagem:**

1. **Proteção de marca**:  
   
   - Evita que terceiros registrem a versão `.com` (mais valorizada) no futuro.  
   - Bloqueia variações indesejadas (ex: alguém tentar `bleeding-kernel.com`).  

2. **Separação de funções**:  
   
   - **`.com`**: Use para landing page pública (ex: exibir status do servidor, documentação técnica).  
   - **`.cc`**: Use exclusivamente para serviços internos (Grafana, VPN, SSH, Portainer).  

3. **Segurança reforçada**:  
   
   - Mantenha serviços críticos **isolados** em um domínio diferente (reduz risco de exposição acidental).  
   - O `.cc` via Cloudflare permite camadas extras de proteção (ex: *Access Policies* para restringir IPs).  

4. **Custo-benefício**:  
   
   - `.com` (~$12/ano) + `.cc` ($8/ano) = **~$20/ano** por dois domínios estratégicos.  

---

### ⚠️ **Ações críticas ao configurar:**

| **Domínio**            | **Recomendações**                                                                                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **bleedingkernel.com** | - Configure **redirect** para uma página estática (ex: "My Homelab Project")<br>- Não exponha serviços aqui!                                                                                                  |
| **bleedingkernel.cc**  | - Habilite **Cloudflare Proxy** (ícone laranja) para ocultar seu IP real<br>- Use **subdomínios** dedicados:<br>  → `grafana.bleedingkernel.cc`<br>  → `vpn.bleedingkernel.cc`<br>  → `nas.bleedingkernel.cc` |

---

### 🔧 **Passo a passo para implantar:**

1. **Registre ambos os domínios** (sugiro Namecheap para `.com` e Cloudflare para `.cc`).  
2. **Na Cloudflare**:  
   - Adicione `bleedingkernel.cc` > DNS > Crie registros `A` ou `CNAME` apontando para seu IP.  
   - Ative **WAF** e **Zero Trust** (gratuito para acesso autenticado ao Grafana).  
3. **No seu servidor**:  
   - Use um proxy reverso (Nginx/Caddy) para rotear subdomínios aos serviços.  
   - Exemplo de configuração Caddy:  
     
     ```nginx
     grafana.bleedingkernel.cc {
         reverse_proxy localhost:3000
     }
     ```

---

### 🚨 **Cuidado com armadilhas:**

- **NUNCA** exponha serviços sensíveis no `.com` (o TLD é mais visado por ataques).  
- Monitore logs de acesso: bloqueie tentativas de força bruta no `.cc` via Cloudflare.  
- Renove os domínios com **antecedência** (ative auto-renew na Cloudflare).  

---

### 💡 **Por que isso é melhor que ".com.br"?**

- **Privacidade total**: Sem vínculo com CPG (dados protegidos pelo WHOIS Privacy da Cloudflare).  
- **Infraestrutura moderna**: Integração nativa com CDN, firewall e certificados SSL automáticos.  
- **Internacionalização**: `.com` + `.cc` são universalmente reconhecidos (ao contrário do `.br`).  

---

# Conclusão

**Compre AMBOS dominios (bleedingkernel.com e bleedingkernel.cc)!** É um investimento mínimo para ganhar flexibilidade, segurança e profissionalismo. Enquanto o `.com` protege sua identidade pública, o `.cc` será seu "santuário técnico". Comece configurando hoje mesmo! 🔥

Se precisar de ajuda com a configuração do Cloudflare Zero Trust ou Caddy, é só pedir.

**Recomendo fortemente a Opção 1 (ambos na Cloudflare):**  

- **Custo total atual é menor** (R\$102,12 vs R\$111,52)  
- **Previsibilidade superior** (só varia com câmbio, não com políticas da Squarespace)  
- **Benefícios técnicos** valem a "aposta" cambial  

Se optar por Cloudflare:  

1. Compre os dois domínios juntos  
2. Configure renovação automática  
3. Monitore câmbio 1x/mês para comprar USD em baixa  

> "Dólar a R$6,50 ainda deixaria o conjunto mais barato que alternativa bruta da Squarespace, e com vantagens técnicas imbatíveis." 

Precisa de ajuda com a cotação de dólar ou configuração? Posso sugerir ferramentas! 💻
