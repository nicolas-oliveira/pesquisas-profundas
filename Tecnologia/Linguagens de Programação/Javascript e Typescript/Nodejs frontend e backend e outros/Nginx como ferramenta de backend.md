# Nginx como ferramenta de backend

**Nginx** não é uma ferramenta que opera dentro do Node.js e nem é um framework, mas sim um **servidor HTTP**, **proxy reverso**, **balanceador de carga** e **cache de conteúdo**[^1]. Em um inventário de ferramentas de backend Node.js, ela se enquadra no tipo:

**Servidor HTTP e Proxy Reverso**
Ferramentas que expõem aplicações ao cliente (navegador ou API), gerenciam conexões HTTP e podem encaminhar (“proxy”) requisições para servidores de aplicação internos, além de oferecer recursos de balanceamento e cache.

Ou seja, é uma ferramenta que opera fora do Node.js. Não dentro do Node.js como o Nest.js.

## Por que Nginx não foi citado no artigo anterior

1. **Âmbito do artigo**
   O artigo anterior focou em bibliotecas e frameworks *dentro* do ecossistema Node.js (ORMs, routers, validação, autenticação, logging etc.). Nginx opera **fora** da aplicação Node, atuando como camada de infraestrutura de entrada.

2. **Classificação distinta**
   Enquanto ferramentas como Express, Koa e NestJS são frameworks de roteamento e middleware que **rodam dentro** do processo Node.js e definem lógica de API, o Nginx é um serviço **externo** que:
- Escuta requisições na porta 80/443

- Encaminha-as (via `proxy_pass`) ao servidor Node.js

- Faz cache de respostas estáticas

- Balanceia carga entre múltiplas instâncias
3. **Não faz parte da pilha de código da aplicação**
   Bibliotecas de roteamento, ORM e validação são dependências instaladas via npm e importadas diretamente no código. Nginx é instalado e configurado no servidor ou container, sendo gerenciado separadamente.
   
   | Tipo de Ferramenta                  | Onde roda         | Exemplo de uso                               |
   |:----------------------------------- |:----------------- |:-------------------------------------------- |
   | Framework HTTP (Express/NestJS)     | Dentro do Node.js | Definição de rotas e middleware de aplicação |
   | Servidor HTTP/Proxy Reverso (Nginx) | Serviço externo   | `proxy_pass http://localhost:3000;`[^2]      |

Dessa forma, **Nginx não consta** na lista de bibliotecas Node.js porque pertence a uma camada de infraestrutura diferente — a **entrada de tráfego e balanceamento** — e não ao **código da aplicação** em si.

<div style="text-align: center">⁂</div>

[^1]: https://nginx.org/en/

[^2]: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/

[^3]: https://itnext.io/how-to-serve-your-backends-with-nginx-a-comprehensive-guide-c8a74955c6ed

[^4]: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-setup-Nginx-reverse-proxy-servers-by-example

[^5]: https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/

[^6]: http://nginx.org/en/docs/beginners_guide.html

[^7]: https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-reverse-proxy-on-ubuntu-22-04

[^8]: https://www.digitalocean.com/community/tutorials/nginx-reverse-proxy-node-angular

[^9]: https://www.digitalocean.com/community/tutorials/how-to-target-your-users-with-nginx-analytics-and-a-b-testing

[^10]: https://www.scaleway.com/en/docs/tutorials/nginx-reverse-proxy/

[^11]: https://itnext.io/how-to-serve-your-backends-with-nginx-a-comprehensive-guide-c8a74955c6ed?gi=b9f5d00f76cb

[^12]: https://dev.to/shubhamkcloud/understanding-reverse-proxy-with-nginx-step-by-step-guide-18a0

[^13]: https://serverfault.com/questions/422152/nginx-how-to-get-status-of-webservers-on-the-backend/422160

[^14]: https://www.packtpub.com/en-us/learning/how-to-tutorials/using-nginx-reverse-proxy

[^15]: https://blog.kronis.dev/blog/how-to-use-nginx-to-proxy-your-front-end-and-back-end

[^16]: https://www.youtube.com/watch?v=lZVAI3PqgHc

[^17]: https://www.solo.io/topics/nginx

[^18]: https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-web-server-and-reverse-proxy-for-apache-on-one-ubuntu-18-04-server

[^19]: https://docs.nginx.com/nginx-gateway-fabric/how-to/traffic-security/securing-backend-traffic/

[^20]: https://www.youtube.com/watch?v=7FpSPSlJj-0
