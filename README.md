# RESUMO

Abordando aqui o necessário e o que lembrar ao criar um projeto Spring seguindo os ensinamentos API RESTful.

## Base Teórica
1. Lembrar sobre [Inversão de Controle](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#bom-primeiro-vamos-lembrar-sobre-invers%C3%A3o-de-controle)
2. [Injeção de Dependência](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#inje%C3%A7%C3%A3o-de-depend%C3%AAncia)
3. [Ciclo de vida do Bean](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#beans) (se será Singleton, Prototype, Request ou Session)
<hr>

## Prática

1. Configuração de Beans passando as anotações. Todas estão [aqui](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#principais-anota%C3%A7%C3%B5es-spring)
2. Lembrar da [arquitetura rest](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#-arquitetura-rest) e suas 6 constraints obrigatórias
<hr>

## O que lembrar ao ir criando Objetos?

Lembrar de dentro da pasta .spring (dentro de java), criar os packages:

### models
Onde ficará as entidades.

**Entidades** receberão os atributos que veremos na UML do projeto ou no
que for solicitado.
O que é interessante inserir:
1. @Entity(name="users");
2. @Table(name="users") - Se o arquivo chama User o nome da table é sempre plural;
3. @Getter & @Setter;
4. @AllArgsConstructor e se quiser noArgs também;
5. @EqualsAndHashCode(of="aqui o que queremos, id por exemplo").

#### Anotações dos Atributos

Devemos olhar os atributos e a UML e verificar se há alguma relação bidimensional.
(ManyToMany, OneToOne...).

Se tiver que implementar algum ENUM, é só criar o arquivo e passar no atributo com @Enumetered

#### ID
- Pode ser do tipo Long ou UUID;
- Passar @ID e o @GenerationValue (.IDENTIY ou .AUTO)

#### Outros Campos
Se quisermos que um campo específico (como CPF ou Email seja unico), passaremos
@Column(unique=true)
<hr>

### repositories
Para persistirmos (salvarmos/encontrarmos) usuário ou algo específico.

O repository conta com diversos métodos prontos para manipulação de tabelas, para não precisarmos ficar implementando.

Ele terá contato direto com o banco de dados.

Criaremos uma interface com o nome do item + repository: ProductRepository e extenderemos
com JpaRepository<Entidade, Identificador>.

Passar anotação @Repository e só.

[Exemplo de métodos pronto.](https://github.com/zenonxd/spring-data-jpa-2024/blob/c95c6fce7a07d3c59fd97a300b0b661a08de2364/src/main/java/com/bookstore/jpa/repositories/BookRepository.java#L11)
Da para passar até query SQL e procurar algo por titulo da entidade.
<hr>

### dtos (caso o projeto tenha algum input de dados de cliente etc)
O Dto serve somente para receber o que está vindo na requisição do corpo em formato JSON.
Em suma, é o que o cliente passará no postman, por exemplo.

O dto já possui métodos prontos getters/constructores/equals&hashcode etc.
Mas não possui Set, pois records são imutáveis.

Se atentar ainda, as anotações dentro do parâmtro, @NotBlank/@NotNull, etc.

[Para ver mais](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#mapeamento-dto-de-entrada-com-records)
<hr>

### services
Regras de negócio!
**Aqui faremos validação e verificação de métodos que iniciamos no repository.**

**Terá acesso aos repositories (iremos importa-los) ou a outras classes do tipo Service** com autowired.

É a camada intermediária da arquitetura MSC, responsável por abstrair as regras de negócio
e controlar o acesso aos dados.

Isso deixará a camada model mais leve e objetiva. 

**Essa camada também será responsável pelo acesso aos dados, validará se as informações recebidas 
do Controller são suficientes para completar a requisição.**

- Centralizar o acesso aos dados e funções externas;
- Abstrair regras de negócios;
- Não ter nenhum "conhecimento" sobre a camada Model (EX: Query SQL);
- Não receber nada relacionada ao HTTP (Request ou Response).
<hr>


### controller
Interação do client com o nosso backend.
**Para chamarmos os métodos criados.**

Importar o service (onde dentro do service teremos os repositories).

Aqui receberemos as requisições do cliente e passaremos adiante ao Service somente as informações
relevantes para completar a requisição.

Essa camada é o "primeiro contato" com as requisições. Enviaremos também ao cliente a resposta
(positiva ou negativa).

- Realizar apenas operações relacionadas a Request e Response (HTTP);
- Não possuir "conhecimento" sobre regras de negócios, ou acesso ao DB;
- Formada quase que exclusivamente por Middlewares.

Precisamos passar o @RequestMapping com a url onde iremos realizar as operações.

[METÓDO GET](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#m%C3%A9todo-get)

[MÉTODO POST](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#m%C3%A9todo-post)

[MÉTODO PUT(UPDATE)](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#m%C3%A9todo-put-update)

[MÉTODO DELETE(UPDATE)](https://github.com/zenonxd/springboot-michelli?tab=readme-ov-file#m%C3%A9todo-del-delete)
<hr>


