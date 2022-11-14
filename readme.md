# Team Style Guide - Corações da Lila

## 1. Criação de branch
Crie a branch baseada no número do seu card no Trello

Código da tarefa + / + nome-do-card
```
US0xx/nome-do-card
```

**Exemplo**

```
US028/websocket-controller-service-model-sala
US028/implementar-websocket
```
<br>

**\*OBSERVAÇÃO: Cuide para sempre criar a branch à partir da branch de desenvolvimento**
<br>

## 2. Configurações do Banco de Dados
Banco/versão:  postegresSQL latest (versão mais recente)
```
User: lila

Senha: lila

Database: coracoes_da_lila
```

## 3. Padrões de escrita de código

- Apenas os termos relacionados ao negócio devem ser mantidos em *PT-BR, sem abreviações.* Verbos e demais termos não relacionados ao negócio, devem ser mantidos em *EN-US*.

**Exemplo**

```Java
...
public List<DocumentoDTO> findAllDocumentos() {
    return this.service.findAllDocumentos();
}
...
public void audit(final User user) {  
    auditConta(contaService.findByUser(user);  
}
...
```
<br>

- Usar Optionals até a camada de controllers

**Exemplo**

```Java

//No service
	@Override
	public Optional<CartaInicio> findById(UUID id) throws DataAccessException {
		return cartaInicioRepository.findById(id);
	}

//No Controller
@GetMapping("/{id}")
    public ResponseEntity<CartaInicio> findCartaInicio(@PathVariable UUID id){
    	
    	Optional<CartaInicio> cartaInicio;
    	cartaInicio=cartaService.findById(id);
    	
    	if (cartaInicio.isEmpty()) {
    		return new ResponseEntity<CartaInicio> (HttpStatus.NOT_FOUND);		
		}
    	return new ResponseEntity<CartaInicio>(cartaInicio.get(),HttpStatus.OK);
    }
```

- Usar @NonNull ao invés de usar dentro do @Column a propriedade nullable = false




## 4. Nomeando métodos

Ao nomear um método, deve-se garantir que seu nome seja auto explicativo e que o verbo seja condizente com o que o método faz.

### Exemplos

### Find (findAll, findById, findOne)

O verbo *find* (do Inglês encontrar ou achar) deve ser empregado quando o intuito do método for **trazer informações ainda não presentes**, como por exemplo no caso de buscas em bancos de dados ou por acesso a serviços externos.

```Java
...
public List<CategoriaProjection> findAll() {
    return this.repository.findAll();  
}
...

```

### Get (getNom, getVlr, getDocumento)

O verbo *get* (do Inglês obter ou receber) deve ser empregado quando o intuito da função for **acessar informação já presentes**, como por exemplo para acessar propriedades de objetos ou realizar pequenas manipulações a alguma propriedade da classe.

```Java
...
public String getDescricao() {  
   return this.descricao;  
}
...

```

### Add

O verbo *add* (do Inglês adicionar ou acrescentar) deve ser empregado quando o intuito da função for **acrescentar algo a um objeto já existente**, como por exemplo para inserir um valor em uma lista ou a uma String.

```Java
...
public void addContentToDescricao(final String content) {
   this.descricao.append(content).append(QUEBRA_LINHA);  
}
...
```

### Build

O verbo *build* (do Inglês construir) deve ser empregado quando o intuito da função for **construir e criar um objeto**, geralmente complexo, como por exemplo para descrever uma função que gera uma instância usando o padrão Builder.

```Java
private List<Conta> buildConta(final Conta conta, final ContaRequest contaRequest) {
    return request.getContas()
            .stream()
            .map(conta -> Conta.builder()
                    .classificacao(classificacao)
                    .titular(conta.getTitular())
                    .build())
            .collect(Collectors.toList());
}
```

### Set

O verbo *set* (do Inglês definir) deve ser empregado quando o intuito da função for **definir alguma propriedade de um objeto**.

```Java
...
public void setDescricao(final Descricao descricao) {  
    this.descricao = descricao;
}
...
```

### To

O verbo *to* (do Inglês para) deve ser empregado quando o intuito da função for **converter um objeto**, como transformar um *DTO* em entidade e vice-versa.

```Java
...
private Conta toEntity(final ContaRequest request) {
    return Conta.builder()
        .idDocumento(request.getIdDocumento())  
        .flagLegado(BooleanTypeEnum.of(request.getFlagLegado()))  
				// Construir todo o objeto
        .build();  
}
...
```

### Save

O verbo *save* (do Inglês salvar) deve ser empregado quando o intuito da função for **persistir um objeto na base**. O comportamento padrão de um *save* num controller, por exemplo, é:

- Se não receber um ID no *body* do request, criar um novo registro.
- Se receber um ID no *body* do request, atualizar o registro com o ID especificado.

O método save poderá ser usado para chamar métodos auxiliares de inserção (como *create*) ou edição (*update*).

```Java
@Override
public Conta save(final ContaRequest request) {
    return isNull(request.getId()) ? insert(request) : update(request);
}
```

## 5. Testes unitários

Os testes unitários devem ser mantidos em *PT-BR*, garantindo que o nome do teste seja auto explicativo evitando obviedades.

### Pontos a serem observados

- Testar (e tratar) todas as condições (por exemplo: nulo, lista vazia, valores negativos ou zerados)
- Testar retornos exatos (por exemplo: comparar conteúdos e não tamanhos de listas)

### Exemplo

```Java
@Test
public void buscarMunicipiosPorEstadoTeste() {
    final String uf = "SP";
    final String nom = "SAO PAULO";
    final String nomExpected = "Sao Paulo";
	
    final MunicipioDto municipio = MunicipioDto.builder()
    	.codigo("007388")
    	.uf(uf)
    	.nom(nom)
    	.build();
	
	when(localizacaoAcesso.buscarMunicipiosPorEstado(anyString()))
		.thenReturn(List.of(municipio));
	
	final List<MunicipioDto> result = localizacaoService.buscarMunicipiosPorEstado(uf);
	
	assertThat(result)
	    .describedAs("Verifica se a lista de municípios tem a quantidade de registros esperada [%s]", List.of(municipioDto).size())
	    .hasSameSizeAs(Collections.singleton(municipio));
	
	assertThat(result)
	    .describedAs("Verifica se o nome do município está com a formatação esperada init-cap [%s]", nomExpected)
	    .extracting(MunicipioDto::getNom)
	    .containsOnly(nomeExpected);
}
```
