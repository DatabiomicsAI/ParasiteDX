# ParasiteDX — Cloudflare Pages shell

Frontend público de `parasitedx.databiomics.com`.

## Arquitetura

`parasitedx.databiomics.com` → Cloudflare Pages → iframe público → `https://parasitedx.streamlit.app/?embed=true`

A aplicação real e o código Python permanecem no repositório canônico `mattoslmp/ParasiteDX`.

## Segurança

- Não coloque senhas, API tokens, Account IDs sensíveis ou Streamlit Secrets neste repositório.
- Cloudflare Workers AI credentials devem ficar somente em **Streamlit Community Cloud → App settings → Secrets**.
- `_headers` restringe framing e delega microfone somente ao app Streamlit esperado.
- O iframe usa o modo público `?embed=true`.

## Pré-requisito

O app `mattoslmp/ParasiteDX`, branch `main`, arquivo `app.py`, precisa estar publicado e acessível em:

`https://parasitedx.streamlit.app`

Se o Streamlit atribuir outro subdomínio, atualize:
1. `src` do iframe em `index.html`;
2. `frame-src` e `Permissions-Policy` em `_headers`.

## Cloudflare Pages

1. Cloudflare → Workers & Pages → Create → Pages → Connect to Git.
2. Selecione `DatabiomicsAI/ParasiteDX`.
3. Production branch: `main`.
4. Framework preset: `None`.
5. Build command: vazio.
6. Build output directory: `/` (ou raiz do projeto conforme a UI atual).
7. Publique.
8. Em **Custom domains**, adicione `parasitedx.databiomics.com`.

## DNS

### Se `databiomics.com` já usa nameservers Cloudflare
Não crie o subdomínio no Namecheap. O Namecheap fica apenas como registrador.
Adicione o domínio personalizado pelo Cloudflare Pages; o Cloudflare gerencia o DNS correspondente.

### Se `databiomics.com` ainda usa Namecheap BasicDNS
Depois de adicionar o custom domain no projeto Pages, crie no Namecheap:

- Type: `CNAME Record`
- Host: `parasitedx`
- Value: o hostname `*.pages.dev` fornecido pelo projeto Cloudflare Pages
- TTL: `Automatic`

Não aponte o CNAME antes de associar o custom domain ao projeto Pages.

## Teste obrigatório

Após publicar, teste em janela anônima:

- carregamento em `https://parasitedx.databiomics.com`;
- tela beta quando `PARASITEDX_BETA_ACCESS_ENABLED=true`;
- texto e envio;
- upload;
- microfone;
- áudio/TTS;
- logout/login;
- painel admin protegido;
- tema dark/light/colorblind;
- HTTPS válido.
