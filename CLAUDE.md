# Calculadora JADAS à beira do leito

Calculadora em português do Juvenile Arthritis Disease Activity Score para residentes de reumatologia pediátrica. O residente marca as articulações ativas num boneco (71 articulações) e a página mostra juntos o cJADAS-10 (em destaque), o JADAS-10, o JADAS-27 e o JADAS-71. O dono do projeto é médico reumatologista pediátrico, não programador: explique em português, de forma simples, o que cada comando faz antes de rodá-lo, e confirme antes de publicar (`git push`), porque o site é público.

## Estrutura
- `index.html`: página única (HTML + CSS + JS, sem dependências nem build, só a fonte do Google Fonts). Abre direto no navegador. Mesmo visual do JDMAI (`../jdmai`).
- Será publicada em `reumatopedhias/jadas` → https://reumatopedhias.github.io/jadas/ e ligada à página inicial (`../reumatopedhias-inicio`, ver o CLAUDE.md de lá).

## Lógica do cálculo (não alterar sem avisar e justificar)
- Componentes: avaliação global do médico (0–10) + avaliação global do bem-estar pelos pais/paciente (0–10) + contagem de articulações ativas + reagente de fase aguda normalizado (0–10).
- Réguas de 0 a 10 em passos de 0,5 (escala de 21 círculos, Filocamo 2010).
- Contagem: JADAS-10 e cJADAS-10 = qualquer articulação, limitada a 10; JADAS-27 = só as 27 (coluna cervical, cotovelos, punhos, MCF 1–3, IFP 1–5 incluindo a IF do polegar, quadris, joelhos, tornozelos); JADAS-71 = todas.
- 71 articulações: ATM, coluna cervical, esternoclaviculares, acromioclaviculares, ombros, cotovelos, punhos, MCF 1–5, IFP 1–5, IFD 2–5, quadris, joelhos, tornozelos, subtalares, mediotársicas, MTF 1–5, IF dos pododáctilos 1–5. O script confere ao abrir que são 71, com 27 marcadas para o JADAS-27.
- VHS normalizado = (VHS − 20) ÷ 10; PCR normalizada = (PCR em mg/L − 10) ÷ 10 (Nordal 2012); ambos limitados a 0–10. A PCR aceita mg/L ou mg/dL (mg/dL × 10).
- cJADAS-10 não usa reagente de fase aguda (faixa 0–30). JADAS-10 0–40; JADAS-27 0–57; JADAS-71 0–101.
- O escore só aparece quando há avaliação do médico, dos pais e a contagem articular. A contagem só vale se houver ao menos uma articulação marcada ou se o residente tocar em "Nenhuma articulação ativa", para não confundir "não examinado" com zero.
- Valores arredondados para 2 casas antes de comparar com os cortes (evita erro de ponto flutuante, ex.: 0,1 + 1 = 1,1000000001).

## Pontos de corte (ACR 2021, Trincianti et al.; todos "≤")
| Estado | cJADAS-10 oligo | cJADAS-10 poli | JADAS-10 oligo | JADAS-10 poli |
|---|---|---|---|---|
| Doença inativa | ≤ 1,1 | ≤ 2,5 | ≤ 1,4 | ≤ 2,7 |
| Atividade mínima | ≤ 4 | ≤ 5 | ≤ 4 | ≤ 6 |
| Atividade moderada | ≤ 12 | ≤ 16 | ≤ 13 | ≤ 17 |
| Alta atividade | > 12 | > 16 | > 13 | > 17 |
- Oligoartrite persistente → cortes de oligo. Oligoartrite estendida e poliartrite FR negativo → cortes de poli (a coorte de poliartrite do estudo incluiu a oligo estendida; aviso na tela e no rodapé).
- Poli FR positivo, sistêmica, entesite, psoriásica, indiferenciada → só o número, sem classificação.
- JADAS-27 e JADAS-71 → só o número (sem cortes validados).

## Caso de teste (botão "Exemplo")
Poliartrite FR negativo; médico 4; pais 3,5; VHS 45 mm/h (→ 2,5); 12 articulações ativas: joelho D, joelho E, punho D, tornozelo E, IFP2 D, cotovelo E, quadril D (7 das 27) + MTF1 D, IFD3 E, ATM D, ombro D, subtalar D.
→ cJADAS-10 = 17,5 (alta atividade); JADAS-10 = 20 (alta); JADAS-27 = 17; JADAS-71 = 22.
Trocando para PCR 2,5 mg/dL (→ 1,5): JADAS-10 = 19; JADAS-27 = 16; JADAS-71 = 21; cJADAS-10 não muda.
Limite: oligo persistente, médico 0,5, pais 0,5, nenhuma articulação, VHS 24 → cJADAS-10 = 1 e JADAS-10 = 1,4, ambos "doença inativa".

## Decisões de conteúdo
- Tradução livre, sem validação transcultural. Manter esse aviso na página.
- Não guardar dados de pacientes (sem localStorage nem envio para servidor).
- Pergunta aos pais: "Considerando todas as formas como a artrite afeta seu filho, como ele está neste momento?" (0 = muito bem, 10 = muito mal).
- Próximos passos planejados pelo dono: sJADAS (AIJ sistêmica) e JSpADA (artrite relacionada à entesite).

## Referências
- Consolaro A et al. Arthritis Rheum 2009;61:658–66. doi:10.1002/art.24516
- McErlane F et al. Ann Rheum Dis 2013;72:1983–8. doi:10.1136/annrheumdis-2012-202031
- Nordal EB et al. Ann Rheum Dis 2012;71:1122–7. doi:10.1136/annrheumdis-2011-200237
- Filocamo G et al. J Rheumatol 2010;37:1534–41. doi:10.3899/jrheum.091474
- Trincianti C et al. Arthritis Rheumatol 2021;73:1966–75. doi:10.1002/art.41879
