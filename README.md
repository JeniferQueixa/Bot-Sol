
<div class="card">
  <h1>☀️ SOL</h1>

  <p>
    A <strong>Sol</strong> foi desenvolvida para automatizar respostas relacionadas à usabilidade da LG e processos operacionais de RH, integrando IA para melhorar o tempo e a qualidade das respostas.
  </p>
</div>

<div class="card">
  <h2> Objetivo</h2>
  <ul>
    <li>Reduzir o número de chamados relacionados a dúvidas frequentes e simples sobre a LG, especialmente em temas cobertos pela biblioteca de POPs.</li>
    <li>Aprimorar a precisão e a confiança das respostas dadas aos colaboradores.</li>
    <li>Automatizar consultas e respostas que anteriormente exigiam intervenção humana.</li>
  </ul>
</div>

<div class="card">
  <h2> Tecnologias Utilizadas</h2>
  <ul>
    <li><strong>Blip:</strong> Plataforma de construção e gerenciamento do bot, com integração ao Slack.</li>
    <li><strong>Slack:</strong> Canal de fácil acesso para o uso da Sol pelos colaboradores.</li>
    <li><strong>LG:</strong> Integração para consumo de dados da folha de pagamento.</li>
    <li><strong>OpenAI:</strong> Implementação do agente de IA utilizando o modelo o4-mini para processamento e geração de respostas.</li>
  </ul>
</div>

<div class="card">
  <h2>📝 Como Usar</h2>
  <ol>
    <li>No Slack, abra o chat <strong>@Sol</strong>;</li>
    <li>Digite um comando (ex: “oi”) e o bot exibirá o menu;</li>
    <li>Escolha um botão no menu para que o bot execute a chamada na LG e apresente os dados requisitados;</li>
    <li>Se o comando não estiver relacionado a dados do colaborador, a Sol identificará a intenção e direcionará a pergunta para o modelo de IA;</li>
  </ol>
</div>

<div class="card">
  <h2> Exemplos de Comandos</h2>
  <table>
    <thead>
      <tr>
        <th>Comando</th>
        <th>Intenção</th>
        <th>Resultado Esperado</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>holerite</td>
        <td>Retorna as folhas disponíveis</td>
        <td>Mostra o resumo do holerite.</td>
      </tr>
      <tr>
        <td>marcar ponto</td>
        <td>Retorna a opção de marcação de ponto</td>
        <td>Realiza a marcação de ponto do colaborador, sem necessidade de acessar a LG.</td>
      </tr>
      <tr>
        <td>espelho de ponto</td>
        <td>Retorna os espelhos disponíveis</td>
        <td>Extrai o cartão de ponto do mês desejado.</td>
      </tr>
    </tbody>
  </table>
</div>

<div class="card">
  <h2> 🤖 Prompt inicial para treinar o modelo</h2>
   <details>
  <div class="prompt-block">
    Você é Solange, mas todos te chamam de Sol, uma agente virtual que apoia os colaboradores da Neon com assuntos de RH de forma acolhedora, clara e leve.
Você é formada em Engenharia de Dados, mas resolveu traçar outros rumos e decidiu se aventurar na área de pessoas. Mora em um motorhome, tem um cachorro chamado Caramelo, ama frio, natureza e praia. Escolheu trabalhar na Neon por conta do seu estilo de vida e da flexibilidade.
Você conversa de forma natural, simpática e próxima, como se fosse parte do time de People Operations (POPS), o time de operações que realiza toda a parte operacional do RH na Neon (processamento de folha, férias, ponto etc.). Sempre que o colaborador precisa de algo, ele entra em contato com você pelo Slack. Caso você não consiga auxiliar, ele é transbordado automaticamente para o atendimento humano.
Sua linguagem é acessível, leve e espontânea, você não soa como um robô, mas como alguém genuinamente disposta a ajudar.
Você responde dúvidas sobre folha de pagamento, ponto eletrônico, férias, benefícios, atestados e outros assuntos do dia a dia operacional de RH. Sempre que possível, oriente com clareza o que a pessoa precisa fazer, explicando com naturalidade.
Evite frases decoradas ou automáticas; varie seu jeito de responder e adapte sua fala à conversa.
Você tem acesso a uma base de conhecimento interna com políticas, CCTs e orientações da Neon. Quando uma pergunta estiver relacionada ao que está nessa base, consulte essas informações antes de responder. Traga a resposta de forma humana e interpretada, como se estivesse explicando para alguém do seu time.
Se a informação não estiver clara ou não existir, diga isso com naturalidade e oriente que o colaborador entre em contato como time de POPs.
Importante:
Você não pode se oferecer para realizar nenhum procedimento pelo colaborador, como registrar, aprovar, alterar ou enviar algo em seu nome.
Você não pode oferecer encaminhamentos, agendamentos ou intermediações, como “mostrar como falar com POPs” ou “abrir um chamado”.
Seu papel é somente orientar, explicar ou informar o colaborador sobre o que ele pode fazer, nunca executar ou intermediar ações operacionais.
Você também não pode negociar condições de trabalho nem tomar decisões pela empresa.
Quando o tema exigir suporte humano, apenas informe que a pessoa deve abrir chamado com o time de POPs.
  </div>
</div>

<div class="card">
  <h2> Validações das Intenções</h2>
  <p>
    As intenções são validadas por padrões regex para identificar comandos e direcionar corretamente as perguntas dos usuários.
    Se uma intenção não for mapeada, o bot direciona para a IA.
  </p>
  <details>
    <summary>Ver INTENTS (exemplo)</summary>
    <pre><code>
const INTENTS = [
    { intent: "saudacao", patterns: [/^oi$/, /^ola$/, /^e(a[iy])$/, /^bom(dia|tarde|noite)$/, /^salve$/, /^falae?$/] },
    { intent: "despedida", patterns: [/^(concluir)$/, /^(encerra(r|mento))$/, /^(tchau)/, /^(quero)?(sair)$/, /^(ate)(logo|mais)$/, /^(falou)$/, /^((quer((o)|(ia)))|(pode))?((fechar)|(encerrar)|(finalizar))(a)?(conversa)?$/, /^(ateh|flw|vlw)$/, /(ate)(logo)$/] },
    { intent: "ath", patterns: [/^(ath)$/, /^(atend((imento)|(ente)))?(humano)$/, /^(eu)?(preciso)?(de)?(ajuda|suporte)/, /(quero)(falar|conversar)(com)?(um)?(atendente)$/, /^(atendimento)$/] },
    { intent: "espelho_ponto", patterns: [/^(espelho)(de)?(ponto)$/, /(ver)?(resumo|marcac(ao|oes))(\w*)?(ponto)$/, /(pontos?)(do)(mes)$/] },
    { intent: "marcar_ponto", patterns: [/^((bater)|(marca(cao|r))|(registrar))(meu|do|de|o)?(ponto)/, /(bater)(meu)?(ponto)/] },
    { intent: "banco_de_horas", patterns: [/^(saldo)((no|d[eo])(banco))?(de)(horas)$/, /(quantas)?(horas)(\w+)?(no|de)(banco)/, /^(horas)(extras)$/, /^(validar)(meu|minhas?)?(banco|saldo)(de)(ho?ra?s?)/, /^(horas?)(extras?)(\w*)?(tenho|possuo)$/] },
    { intent: "menu_ponto", patterns: [/^(((menu)|(falar))?((de)|(sobre))?)(ponto)$/, /(opcoes|informacoes)?(de|para)?(ponto)$/] },
    { intent: "aviso_ferias", patterns: [/^(aviso)(d[ea]s?)?(minhas?)?(ferias)$/, /^notificar(ferias)$/, /(como|preciso|quero)(posso)?(notificar|avis(o|ar))(\w*)?(ferias)/] },
    { intent: "pagamento_ferias", patterns: [/^(pagamento)(d[ea]s?)?(minhas?)?(ferias)$/, /(quando)(vou)?(receb(o|er))(minhas?)?(ferias)$/] },
    { intent: "marcar_ferias", patterns: [/^(solicitar|agendar|marca(r|cao))(d[ea]s?)?(minhas?)?(ferias)$/, /^pedir(ferias)$/] },
    { intent: "recibo_ferias", patterns: [/^(recibo)(d[ea]s?)?(minhas?)?(ferias)$/, /(comprovante)(das?)?(minhas?)?(ferias)$/] },
    { intent: "saldo_ferias", patterns: [/^(saldo)(d[ea]s?)?(minhas?)?(ferias)$/, /(quantos)(dias)(ainda)?(posso|tenho)?(tirar)?(de)?(ferias)/] },
    { intent: "ferias", patterns: [/^(menu)?(de)?(ferias)$/, /(informacoes)(de|sobre)(ferias)$/, /(funcionam?)(as)?(minhas?)(ferias)$/] },
    { intent: "sugestao", patterns: [/^(dar)?(uma)?(sugest[aã]o)$/, /^avalia(r|cao)$/, /^deixar(opiniao)$/, /(como|posso)(d(eix)?ar)(uma?)(ideia|feedback)$/] },
    { intent: "dados_cadastrais", patterns: [/^((me)(mostra)|(quero)?(ver)?)(meus)?(dados)(de)?(cadastr(o|ais))$/, /(minhas)(informacoes)(pessoais)$/] },
    { intent: "holerite", patterns: [/^(onde)?(posso|quero)?(baix(o|ar)|acessar|ve(jo|r))?(meu)?(holerite|contracheque|(quanto)(vou)(receber))/, /^(extrato)?(salario)(do)?(mes)?/, /^((me)(envia|manda)(meu)?(holerite|contracheque))$/] },
    { intent: "calendario_pagamento_holerite", patterns: [/^(quero)?((saber)(mais)|ver)?(meu|minha|o)?(calendario|data)(de)?(pa?ga?m?e?n?to?s?)$/, /^(quando)(recebo|cai)(\w*)?(salario)$/, /(data)(\w*)?(proximo)(pa?ga?m?e?n?to?)/] },
    { intent: "dados_de_folha", patterns: [/^(onde)?(posso|quero)?(ver|acessar)?(meus|minha)?((dados)(d[ae])?(folha))|((folha)(de)(pa?ga?m?e?n?to?s?))$/, /^(consult(o|ar))(meu|minha)?(contracheque|(folha)(\w*)?(pa?ga?me?nto?))$/] },
];

function run(input) {
    if(input.includes("cameFrom_aifaq_regexIntent")) {
        input = input.split("cameFrom_aifaq_regexIntent:")[1];
    }

    for (const { intent, patterns } of INTENTS) {
        if (patterns.some((regex) => regex.test(textNormalizer(input)))) {
            return intent;
        }
    }
    return false;
}

function textNormalizer(dataToNormalize) {
    return dataToNormalize
        .normalize("NFD")
        .replace(/([^ªºa-zA-Z0-9]+)/g, "")
        .toLowerCase();
}
    </code></pre>
  </details>
</div>

<div class="card">
  <h2> Requisição na OpenAI</h2>
  <details>
    <summary>Ver Código</summary>
    <pre><code>
function run() {
    return {
        "model": "o4-mini",
        "input": [
            {
                "role": "developer",
                "content": [
                    {
                        "type": "input_text",
                        "text": "{{resource.promptSol}}"
                    }
                ]
            },
            {
                "role": "system",
                "content": "Responda de forma objetiva."
            },
            {
                "role": "user",
                "content": [
                    {
                        "type": "input_text",
                        "text": "{{firstContent}}"
                    }
                ]
            }
        ],
        "text": {
            "format": {
                "type": "text"
            },
            "verbosity": "medium"
        },
        "reasoning": {
            "effort": "medium",
            "summary": "auto"
        },
        "tools": [
            {
                "type": "file_search",
                "vector_store_ids": ["{{config.vectorId}}"]
            }
        ],
        "store": true,
        "include": [
            "reasoning.encrypted_content",
            "web_search_call.action.sources"
        ]
    }
}
    </code></pre>
  </details>
</div>

<div class="card">
  <h2> Histórico de Intenções </h2>
  <details>
    <summary>Ver Código</summary>
    <pre><code>
function run(payload, response, userInput) {
    const DEFINITIONS = {
        "model": "o4-mini",
        "firtsInput": {
            "role": "developer",
            "content": [
                {
                    "type": "input_text",
                    "text": "{{resource.promptSol}}"
                }
            ]
        },
        "secondInput": {
            "role": "system",
            "content": "Responda de forma objetiva."
        },
        "text": {
            "format": {
                "type": "text"
            },
            "verbosity": "medium"
        },
        "reasoning": {
            "effort": "medium",
            "summary": "auto"
        },
        "tools": [
            {
                "type": "file_search",
                "vector_store_ids": ["{{config.vectorId}}"]
            }
        ],
        "store": true,
        "include": [
            "reasoning.encrypted_content",
            "web_search_call.action.sources"
        ]
    }

    const inputAI = userInput.includes("concatPayloadAI:") ? userInput.split("concatPayloadAI:")[1] : userInput;
    try {
        const { model, firtsInput, secondInput, text, reasoning, tools, store, include } = DEFINITIONS;
        payload = JSON.parse(payload);

        payload.model = model;
        payload.input[0] = firtsInput;
        payload.input[1] = secondInput;
        payload.text = text;
        payload.reasoning = reasoning;
        payload.tools = tools;
        payload.store = store;
        payload.include = include;

        const modelAssistant = {
            "role": "assistant",
            "content": [
                {
                    "type": "output_text",
                    "text": response
                }
            ]
        };
        const modelUser = {
            "role": "user",
            "content": [
                {
                    "type": "input_text",
                    "text": inputAI
                }
            ]
        };

        payload.input.push(modelAssistant, modelUser);

        const MAX_INTERACTION = 30;
        const LOWER_EDGE = (payload.input.length - MAX_INTERACTION);
        if (payload.input.length > MAX_INTERACTION) {
            const newPayloadInput = [payload.input[0], ...payload.input.slice(LOWER_EDGE + 1,)];
            payload.input = newPayloadInput;
        }

        return payload;
    } catch (error) {
        const { model, firtsInput, secondInput, text, reasoning, tools, store, include } = DEFINITIONS;
        return {
            model,
            "input": [
                firtsInput,
                secondInput,
                {
                    "role": "user",
                    "content": [
                        {
                            "type": "input_text",
                            "text": inputAI
                        }
                    ]
                }
            ],
            text,
            reasoning,
            tools,
            store,
            include
        };
    }
}
    </code></pre>
  </details>
</div>

</body>

</html>

