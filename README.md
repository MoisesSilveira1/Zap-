# Enviar Script - Documentação

Este repositório contém a função `enviarScript`, projetada para automatizar o envio de mensagens linha a linha em uma interface de conversa baseada no DOM. Esta funcionalidade é útil em cenários onde deseja-se interagir automaticamente com elementos de uma página, como simulando interações humanas em interfaces web.

## Visão Geral

A função `enviarScript` divide um texto em linhas, insere cada linha em uma área de texto de conversa, e envia a mensagem utilizando os controles DOM fornecidos pela interface. Há suporte para configurar um atraso entre os envios, simulando uma interação mais realista.

## Código

```javascript
async function enviarScript(scriptText, delay = 1000) {
    const lines = scriptText.split('\n').map(line => line.trim()).filter(line => line);

    const main = document.querySelector("#main");
    const textarea = main.querySelector(`div[contenteditable="true"]`);

    if (!textarea) {
        throw new Error("Não há uma conversa aberta");
    }

    for (const line of lines) {
        console.log(line);
        textarea.focus();
        document.execCommand('insertText', false, line);
        textarea.dispatchEvent(new Event('change', { bubbles: true }));

        await new Promise(resolve => setTimeout(resolve, 100));

        const sendButton = main.querySelector(`[data-testid="send"]`) || main.querySelector(`[data-icon="send"]`);
        sendButton.click();

        if (lines.indexOf(line) !== lines.length - 1) {
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }

    return lines.length;
}

const script = `

xxxxxxxxx (coloque sua mensagem aqui) xxxxxxxxxxxxx
xxxxxxxxx (coloque sua mensagem aqui) xxxxxxxxxxxxx
xxxxxxxxx (coloque sua mensagem aqui) xxxxxxxxxxxxx

`;

enviarScript(script, 2000)
    .then(e => console.log(`Código finalizado, ${e} mensagens enviadas`))
    .catch(console.error);
```

## Parâmetros

- `scriptText` (string): O texto que será enviado, com cada linha separada por uma quebra de linha (`\n`).
- `delay` (number): Tempo em milissegundos entre o envio de cada mensagem. O padrão é 1000 ms (1 segundo).

## Como Usar

1. Inclua o código no console do navegador ou em um script que tenha acesso à interface DOM da página desejada.
2. Substitua o valor de `script` pelo texto desejado, garantindo que as mensagens estejam separadas por quebras de linha.
3. Execute a função `enviarScript`.

## Exemplo de Uso

```javascript
const script = `
Olá!
Como você está?
Isso é um teste automatizado.
`;

enviarScript(script, 1500)
    .then(e => console.log(`Código finalizado, ${e} mensagens enviadas`))
    .catch(console.error);
```

Neste exemplo, cada linha será enviada com um intervalo de 1500 milissegundos entre as mensagens.

## Requisitos

- A página deve conter uma área de texto com o atributo `contenteditable="true"` e um botão de envio com um dos seletores `[data-testid="send"]` ou `[data-icon="send"]`.
- É necessário ter permissão para executar scripts no navegador.

## Erros Comuns

- **"Não há uma conversa aberta"**: Ocorre quando a função não encontra a área de texto na página. Certifique-se de estar na interface correta.
- **Botão de envio não encontrado**: Verifique os seletores utilizados para localizar o botão de envio e atualize conforme necessário.

## Contribuições

Contribuições são bem-vindas! Se encontrar problemas ou tiver sugestões de melhoria, sinta-se à vontade para abrir uma issue ou enviar um pull request.
