# Checkpoint 5 — Bug Hunt PetFiap

> Copie este arquivo para a raiz do seu repositório com o nome **README.md**
> e preencha todas as seções.

## Identificação

**Grupo:** ___

| Integrante | RM | Turma |
|---|---|---|
|Amom Ianaguivara Brito |565718 |2CCPH |
|Victor Chen |565363|2CCPH |
|Fernando Antônio |	562549 |2CCPH |

| Campo | |
|---|---|
| **Total de bugs corrigidos** | 3 / 12 |
| **Total de ajustes de Clean Code** | 0 / 6 |
| **Total de testes novos escritos** | 0/ 6 |
| **Suíte final (Run As → JUnit Test)** | ___ testes, ___ falhas |

---

## Parte 1 — Bugs encontrados

> Uma linha por bug, na ordem em que você os encontrou. Use a numeração dos seus
> commits (`fix: bug01 ...`). Preencha TODAS as colunas — metade da nota está aqui.

| # | Sintoma observado (o que fiz/vi) | Causa raiz (arquivo e linha aproximada) | Correção aplicada | Conceito da disciplina |
|---|---|---|---|---|
| bug01 | Os testes mostravam que GeradorProtocolo.getInstancia() retornava instâncias diferentes e que os protocolos não mantinham uma sequência global. | Em GeradorProtocolo.java, aproximadamente nas linhas 17–21, o método getInstancia() criava um novo GeradorProtocolo quando instancia era null, mas não armazenava o objeto no atributo estático instancia. | O novo objeto passou a ser atribuído a instancia antes do retorno, garantindo que as chamadas seguintes reutilizem o mesmo objeto. | Padrão de projeto Singleton: garante uma única instância compartilhada da classe e, neste caso, preserva o contador global de protocolos. |
| bug02 | O teste deveCriarTosaQuandoTipoForTosa mostrava que, ao solicitar um atendimento do tipo TOSA, o objeto criado era da classe Banho. | Em AtendimentoFactory.java, no case "TOSA" do método criar(), a Factory instanciava new Banho(...) em vez de new Tosa(...). | A instanciação do case "TOSA" foi alterada para new Tosa(...), fazendo a Factory criar a subclasse correspondente ao tipo solicitado. | Padrão de projeto Factory: centraliza a criação dos objetos concretos e deve selecionar corretamente a implementação de acordo com o tipo recebido. |
| bug03 | O teste devePreencherOsDadosDoPetNaConsulta mostrava que uma consulta criada pela Factory retornava null para dados como nome e porte do pet. | Em ConsultaVeterinaria.java, aproximadamente na linha 17, o construtor recebia os dados do atendimento, mas chamava apenas super(), deixando os atributos herdados de Atendimento sem inicialização. | O construtor passou a chamar super(protocolo, petNome, petPorte, tutorNome, dataHora), repassando os dados para o construtor da classe pai. | Herança e chamada de construtor com super(...): a subclasse deve inicializar corretamente o estado herdado da superclasse. |
| bug04 | | | | |
| bug05 | | | | |
| bug06 | | | | |
| bug07 | | | | |
| bug08 | | | | |
| bug09 | | | | |
| bug10 | | | | |
| bug11 | | | | |
| bug12 | | | | |

## Parte 2 — Ajustes de Clean Code

| # | Onde estava | Qual princípio/boas práticas era violado | O que eu mudei |
|---|---|---|---|
| clean01 | | | |
| clean02 | | | |
| clean03 | | | |
| clean04 | | | |
| clean05 | | | |
| clean06 | | | |

## Parte 3 — Testes novos (regras que estavam sem cobertura)

> Uma linha por teste novo (`test: ...`). "Regra coberta" é o comportamento do
> contrato (seção 3 do enunciado) que o teste protege. Em "Resultado", diga se o
> teste ficou vermelho ao ser escrito (revelou bug — qual?) ou verde de cara
> (regra já estava correta).

| # | Teste escrito (classe.método) | Regra coberta | Resultado ao escrever (vermelho/verde) |
|---|---|---|---|
| teste01 | | | |
| teste02 | | | |
| teste03 | | | |
| teste04 | | | |
| teste05 | | | |
| teste06 | | | |

---

## Parte 4 — Perguntas de reflexão

> Responda com suas palavras, 5 a 10 linhas cada, **usando o código real do
> projeto como exemplo**. Respostas genéricas de tutorial não pontuam.

### 1. A suíte como contrato (Aula 15)
O projeto chegou com 20 testes, 9 vermelhos. Descreva como você usou as
mensagens de falha (ex.: `expected: <Rex> but was: <null>`) para caçar os bugs.
O que a suíte de testes tem de melhor do que testar tudo na mão com curl?

### 2. Mock e injeção de dependência (Aulas 13 a 15)
No `AgendaServiceTest`, o `@Mock` cria um `AtendimentoRepository` falso e o
`@InjectMocks` o injeta no service. Explique a relação disso com o `@Autowired`
que o Spring faz em produção — quem "injeta" em cada mundo, e por que o teste
consegue rodar sem banco e sem subir o Spring?

### 3. `==` vs `.equals()` (Aula 7)
Um dos bugs fazia o agendamento duplicado passar pela verificação de conflito.
Explique por que `==` entre Strings e `LocalDateTime` falhou aqui, por que ele
"funciona por sorte" com literais como `"Rex"`, e o que a sua correção mudou.

### 4. Sobrescrita vs sobrecarga (Aula 7)
Um dos bugs compilava sem nenhum erro: um método parecia sobrescrever
`getDuracaoMinutos`, mas na verdade criava uma assinatura nova. Explique a
diferença entre override e overload nesse caso e por que a anotação `@Override`
teria impedido o bug.

### 5. Singleton manual vs bean do Spring (Aula 14)
O `GeradorProtocolo` é um Singleton escrito à mão e causou um dos bugs.
Explique o que ele garante, qual foi o bug, e por que o `AgendaService`
(`@Service`) não corre o mesmo risco no container do Spring.

### 6. Cobertura de testes: onde parar? (Aula 15)
Dos 6 testes novos que você escreveu, alguns ficaram vermelhos (revelaram
bugs) e outros verdes de cara (regras já corretas). Vale a pena manter os que
ficaram verdes? Em um projeto real com prazo, o que você priorizaria testar:
caminho feliz, caminhos de erro, ou 100% de cobertura? Justifique.

---

## Parte 5 — Espaço livre (opcional)

Alguma dificuldade, dúvida ou comentário sobre o checkpoint?

```

```
