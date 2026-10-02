# Checkpoint 5 — Bug Hunt PetFiap

> Copie este arquivo para a raiz do seu repositório com o nome **README.md**
> e preencha todas as seções.

## Identificação

**Grupo:** Fiapers

| Integrante | RM | Turma |
|---|---|---|
|Amom Ianaguivara Brito |565718 |2CCPH |
|Victor Chen |565363|2CCPH |
|Fernando Antônio |	562549 |2CCPH |

| Campo | |
|---|---|
| **Total de bugs corrigidos** | 12 / 12 |
| **Total de ajustes de Clean Code** | 0 / 6 |
| **Total de testes novos escritos** | 6 / 6 |
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
| bug04 | O teste deveMontarAtendimentoCompleto mostrava que o nome do pet ficava null mesmo após chamar comPet("Rex", "PEQUENO"). | Em AtendimentoBuilder.java, aproximadamente nas linhas 23–26, o método comPet() fazia petNome = petNome, atribuindo o parâmetro a ele mesmo e deixando o atributo da classe sem valor. | A atribuição foi alterada para this.petNome = petNome, diferenciando o atributo da instância do parâmetro recebido. | Uso de this e encapsulamento de estado: this.petNome referencia o atributo do objeto, enquanto petNome referencia o parâmetro local do método. |
| bug05 | Os testes deveRecusarMontagemSemNomeDoPet e deveRecusarMontagemSemPorte mostravam que o Builder permitia criar atendimentos mesmo sem informações obrigatórias do pet. | Em AtendimentoBuilder.java, no método construir(), o atendimento era enviado diretamente para a Factory sem validar se petNome e petPorte estavam preenchidos. | Foi adicionada uma validação em construir() que lança IllegalArgumentException quando o nome ou o porte do pet são nulos, impedindo a criação de um objeto inválido. | Padrão Builder e validação de estado: o objeto deve ser validado no momento da construção para garantir que somente instâncias válidas sejam criadas. |
| bug06 | O teste deveRecusarAgendamentoComHorarioJaOcupado mostrava que um novo atendimento para o mesmo pet e no mesmo horário era salvo em vez de lançar HorarioOcupadoException. | Em AgendaService.java, no método agendar(), nome do pet e data/hora eram comparados com ==. Como String e LocalDateTime são objetos, == compara referências de memória e não o conteúdo dos objetos. | As comparações foram alteradas para .equals(), fazendo a verificação considerar valores equivalentes mesmo quando estão armazenados em objetos diferentes. | Comparação de objetos em Java: == compara referências, enquanto .equals() compara igualdade de conteúdo conforme a implementação da classe. |
| bug07 | O teste deveLancarExcecaoQuandoAtendimentoNaoExiste mostrava que buscar um ID inexistente retornava null em vez de lançar AtendimentoNaoEncontradoException. | Em AgendaService.java, aproximadamente nas linhas 36–42, o método buscarPorId() lançava corretamente a exceção com orElseThrow(), mas um catch (Exception e) genérico capturava essa exceção e retornava null. | O try/catch genérico foi removido, permitindo que AtendimentoNaoEncontradoException seja propagada normalmente pelo orElseThrow(). | Tratamento de exceções e uso de Optional.orElseThrow(): exceções de negócio não devem ser capturadas e silenciosamente convertidas em valores inválidos como null. |
| bug08 | O novo teste de preço do banho mostrou que pets de porte PEQUENO recebiam preço de 100 Reais e pets GRANDES recebiam R$ 60, contrariando o contrato. | Em `Banho.java`, aproximadamente nas linhas 26–33, o método `calcularPreco()` retornava os valores de PEQUENO e GRANDE invertidos. | Os retornos foram corrigidos para R$ 60 no porte PEQUENO, R$ 80 no MEDIO e R$ 100 no GRANDE. | Polimorfismo e regras de negócio no model: a sobrescrita de `calcularPreco()` deve implementar corretamente o comportamento específico de `Banho`. |
| bug09 | O novo teste `deveDurar60Minutos` mostrava que a Tosa não retornava a duração de 60 minutos definida no contrato. | Em `Tosa.java`, o método foi declarado como `getDuracaoMinutos(String porte)`, enquanto o método herdado não recebe parâmetros. Isso criava uma sobrecarga em vez de sobrescrever o método da classe pai. | A assinatura foi corrigida para `getDuracaoMinutos()` e foi adicionada a anotação `@Override`. | Sobrescrita vs sobrecarga: override mantém a mesma assinatura do método herdado e altera seu comportamento; overload cria outro método com parâmetros diferentes. |
| bug10 | O novo teste `deveRecusarAgendamentoComDataHoraNoPassado` mostrou que um atendimento com data/hora passada não era recusado e o serviço chegava a acessar o repositório. | Em `AgendaService.java`, no início do método `agendar()`, não existia validação da data/hora antes da consulta ao repository. | Foi adicionada uma validação com `isBefore(LocalDateTime.now())` que lança `IllegalArgumentException` antes de qualquer acesso ao repositório. | Validação de regras de negócio e fail-fast: entradas inválidas devem ser rejeitadas o mais cedo possível, evitando processamento e acesso desnecessário à camada de persistência. |
| bug11 | O novo teste `deveRecusarCancelamentoDeAtendimentoConcluido` mostrou que um atendimento com status `CONCLUIDO` podia ser alterado para `CANCELADO`. | Em `Atendimento.java`, aproximadamente nas linhas 62–65, o método `cancelar()` alterava diretamente o status para `CANCELADO` sem verificar o estado atual do atendimento. | Foi adicionada uma validação que permite o cancelamento somente quando o status é `AGENDADO`; nos demais estados é lançada `StatusInvalidoException`. | Encapsulamento de regras de negócio e controle de transição de estado: o próprio model deve impedir mudanças de status inválidas. |
| bug12 | A suíte permanecia verde, mas durante o code review foi identificado que a entidade `Atendimento` não possuía geração automática para sua chave primária. Em uma persistência real, novos atendimentos poderiam ser enviados ao banco com `id` nulo. | Em `Atendimento.java`, aproximadamente nas linhas 14–15, o atributo `id` possuía apenas `@Id`, sem uma estratégia de geração de identificador. | Foi adicionada a anotação `@GeneratedValue(strategy = GenerationType.IDENTITY)` ao atributo `id`, deixando a geração da chave primária sob responsabilidade do banco/JPA. | JPA e persistência de entidades: chaves primárias geradas automaticamente devem declarar uma estratégia de geração com `@GeneratedValue`. |

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
| teste01 | BanhoTest.deveCalcularPrecoCorretoQuandoPorteVariar | O preço do banho deve variar conforme o porte: 60 reais para PEQUENO, 80 reais para MEDIO e 100 reais para GRANDE. | Vermelho — revelou o bug08: os preços dos portes PEQUENO e GRANDE estavam invertidos. |
| teste02 | `TosaTest.deveDurar60Minutos` | A Tosa deve ter duração de 60 minutos. | Vermelho — revelou o bug09: o método de duração da Tosa não sobrescrevia corretamente o método da classe pai. |
| teste03 | `ConsultaVeterinariaTest.deveCustar150ReaisIndependenteDoPorte` | A consulta veterinária deve custar R$ 150,00 independentemente do porte do pet. | Verde de cara — a regra já estava implementada corretamente. |
| teste04 | `AgendaServiceTest.deveRecusarAgendamentoComDataHoraNoPassado` | Um atendimento com data/hora no passado deve lançar `IllegalArgumentException` antes de qualquer acesso ao repositório. | Vermelho — revelou o bug10: o serviço não validava a data/hora antes de consultar e salvar no repositório. |
| teste05 | `AgendaServiceTest.deveRecusarCancelamentoDeAtendimentoConcluido` | Um atendimento já CONCLUIDO não pode ser cancelado e deve lançar `StatusInvalidoException`, sem salvar alterações no repositório. | Vermelho — revelou o bug11: o método `cancelar()` permitia cancelar um atendimento já concluído. |
| teste06 | `AgendaServiceTest.deveCancelarAtendimentoAgendado` | Um atendimento com status `AGENDADO` deve poder ser cancelado, passando para `CANCELADO` e sendo salvo no repositório. | Verde de cara — a regra já estava implementada corretamente. |

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
