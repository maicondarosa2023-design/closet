# Gerador de Catálogo com IA

Site estático (HTML + JavaScript puro): cadastre até 100 produtos com preço e foto, gere o catálogo em PDF, use IA para escrever descrições e envie o PDF para grupos de WhatsApp. Tem aba de histórico com opção de editar e reenviar.

Os dados ficam no navegador de quem usa (localStorage e IndexedDB). Não há banco de dados nem servidor.

## Publicar no GitHub Pages

1. No GitHub, crie um repositório novo (por exemplo `catalogo`), público.
2. Clique em **Add file → Upload files** e envie `index.html` e `README.md` (ou arraste o conteúdo do zip já extraído).
3. Vá em **Settings → Pages**. Em **Build and deployment**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e salve.
4. Aguarde cerca de 1 minuto. O site fica em `https://SEU-USUARIO.github.io/catalogo/`.

## IA gratuita

Abra **⚙️ IA gratuita (configurar)** no topo da página. O site usa qualquer API compatível com OpenAI:

- **Pollinations**: sem chave. É o padrão.
- **Google Gemini**: crie uma chave gratuita no Google AI Studio e cole no campo Chave.
- **Groq**: crie uma chave gratuita no console da Groq e cole no campo Chave.

Serviços gratuitos mudam endereços, modelos e limites com o tempo. Se algum parar de responder, ajuste o endereço e o modelo nos campos, ou troque de serviço. A chave fica salva só no navegador de quem digitou. Nunca coloque chave dentro do código do repositório, porque o site é público.

## Envio para grupos do WhatsApp

Páginas do GitHub Pages não têm servidor, então o navegador não consegue falar com o WhatsApp sozinho. Há dois caminhos:

**1. Webhook (envio automático).** Em *Como o envio funciona*, informe o endereço de um servidor seu que envie pelo WhatsApp (por exemplo um fluxo no n8n ou Make, ou uma API como a Evolution API). O site faz um POST em JSON:

```json
{
  "action": "send_document",
  "to": "ID do grupo cadastrado",
  "filename": "minha-loja.pdf",
  "mimetype": "application/pdf",
  "caption": "mensagem",
  "media_base64": "...PDF em base64..."
}
```

- O botão **Testar servidor** envia `{"action":"ping"}`.
- Se houver token, ele vai no cabeçalho `Authorization: Bearer TOKEN`.
- Responda com status 2xx quando enviar. Qualquer outro status aparece como falha.
- O servidor precisa liberar CORS para o endereço do seu site, incluindo a requisição `OPTIONS` com os cabeçalhos `Content-Type` e `Authorization`.
- O QR de pareamento do WhatsApp é gerado pelo painel do seu servidor. Cole o link desse painel no campo opcional para ver um QR que o abre no celular.

**2. Compartilhar pelo celular.** O botão *Compartilhar pelo celular* abre o menu de compartilhamento do aparelho com o PDF. Você escolhe o grupo manualmente.

Cadastre seus grupos na caixa *Seus grupos*, um por linha, no formato `Nome | ID do grupo`.

Enviar o mesmo arquivo para muitos grupos em sequência pode levar o WhatsApp a limitar o número. O site espera de 4 a 8 segundos entre os envios; comece com poucos grupos.

## Histórico

A aba **Enviados** guarda cada envio com a versão do catálogo daquele momento. Você pode **Reenviar** (editando mensagem e grupos), **Editar** (carrega o catálogo de volta para alterar) ou **Excluir**. O histórico fica só no navegador usado.
