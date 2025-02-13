# REST API Naming Conventions and Best Practices

# 1. Regras de URL 
A URL deve ser legível por humanos. Uma URL bem definida traz uniformidade, fácil descoberta e fácil adoção em toda a empresa.
Use nomes descritivos e significativos.
Escolha nomes claros e concisos que representem com precisão o propósito e a funcionalidade do recurso ou ação.
Evite abreviações ou siglas enigmáticas que podem ser confusas para os consumidores de API.
Existe Limitação de tamanho do URL da API, esteja atento.

## Use substantivos / não verbos para os endpoints

Ao construir sua API RESTful, certifique-se de usar substantivos como seus recursos em vez de verbos. Em vez de criar URLs com endpoints contendo (combinações verbo-substantivo: hífen, snake_case, camelCase):

/createusers
/deleteassociates
/deleteAssociates
/delete-associates
/delete_associates
/updateVendors
/update-vendors

Você deve optar por endpoints RESTful que se pareçam com:
/users
/associates
/vendors

## Recursos Únicos e de Coleção

Um recurso pode ser um único ou uma coleção.
Por exemplo, “customers” é um recurso de coleção e “customer” é um recurso único (em um domínio bancário).
Podemos identificar o recurso de coleção “customers” usando o URI “/customers“. 
Podemos identificar um único recurso “customer” usando o URI “/customers/{customerId}“.

Ex: 
/customers			//É uma coleção
/customers/{id}		//É um recurso único

## Recursos de Coleção e Subcoleção
Um recurso pode conter recursos de subcoleção também.

Por exemplo, o recurso de subcoleção “contas” de um “cliente” específico pode ser identificado usando o URN “/customers/{customerId}/accounts” (em um domínio bancário).

Da mesma forma, um recurso singleton “conta” dentro do recurso de subcoleção “contas” pode ser identificado da seguinte forma: “/customers/{customerId}/accounts/{accountId}“.

## Use Hífen (-) em URL (preferencial)
O uso de um hífen (-) melhora a legibilidade em comparação a outros separadores como sublinhado etc. No caso de um nome longo, pode-se usar um hífen (-).

## Sublinhados (_) devem ser evitados na URL
Sublinhados(_) não são legíveis e frequentemente se misturam com os sublinhados a olho nu.
Em alguns servidores, o DNS não permite o uso de sublinhados(_) na URL base.

## Use o controle de versão na URL
Se você planeja introduzir mudanças drásticas ou atualizações no futuro, considere incluir o controle de versão na URL para garantir a compatibilidade com versões anteriores.

## Evite aninhamento excessivo
URLs de API devem focar no recurso ou ação que está sendo realizada.
Não deve expor detalhes de implementação, como pilha de tecnologia, nomes de banco de dados ou estrutura interna do servidor.

## Use barras (/) para hierarquia, mas não barras finais (/)
As barras são usadas para mostrar a hierarquia entre recursos e coleções individuais.

Evite:
http://api.example.com/v1/store/items/

Bom exemplo:
http://api.example.com/v1/store/items

## Evite usar extensões de arquivo
Elas são desnecessárias e adicionam comprimento e complexidade aos URIs.

Evite:
http://api.example.com/v1/store/items.json
http://api.example.com/v1/store/products.xml

Bons exemplos:
http://api.example.com/v1/store/items
http://api.example.com/v1/store/products

## Use o componente de query para filtrar a coleta de URI
Você frequentemente encontrará requisitos que exigem que você classifique, filtre ou limite um grupo de recursos dependendo de um atributo de recurso específico. 
Em vez de criar APIs adicionais, habilite a classificação, filtragem e paginação na API de coleta de recursos e forneça os parâmetros de entrada como parâmetros de consulta para atender a esse requisito.

http://api.example.com/v1/store/items?group=124
http://api.example.com/v1/store/employees?department=IT&region=USA

## Use letras minúsculas em URIs
Quando conveniente, letras minúsculas devem ser consistentemente preferidas em caminhos de URI.

## Nunca use nomes de funções CRUD em URIs
Não devemos usar URIs para indicar uma função CRUD. URIs devem ser usados ​​apenas para identificar os recursos e não qualquer ação sobre eles exclusivamente.

Devemos usar métodos de solicitação HTTP para indicar qual função CRUD é executada.

## Evite o uso de verbos na URI
Não é correto colocar os verbos em URIs REST. 
REST usa substantivos para representar recursos, e métodos HTTP (GET, POST, PUT, DELETE, etc.) são então usados ​​para executar ações nesses recursos, agindo efetivamente como verbos.

Se usarmos verbos no URI, provavelmente estamos criando uma chamada de método no estilo RPC com um formato de solicitação/resposta JSON ou XML. Seria incorreto chamá-lo de REST.

# 2. Regras de Métodos HTTP
Descreva a funcionalidade do recurso com métodos HTTP

## Métodos GET não devem alterar o estado
O método GET deve ser usado apenas para recuperar registros. Se você precisar alterar o estado, deve usar POST, PUT, DELETE e métodos PATCH menos usados.

## Forneça amplo feedback para ajudar os desenvolvedores a terem sucesso
Algumas APIs adotam uma visão minimalista, retornando apenas os códigos de status HTTP (201-CRIADO ou 204-SEM CONTEÚDO) e, embora isso não seja incorreto, prefira fornecer mensagens de status mais detalhadas como respostas JSON/XML para dar aos usuários das APIs o máximo de informações possível para ter sucesso. Se eles decidirem usá-lo ou não, dependerá inteiramente deles.

## Atualizações e criações retornam uma representação de recurso
Os métodos POST, PUT ou PATCH podem modificar um ou mais campos nos recursos subjacentes. Conforme declarado anteriormente, retornar todos os detalhes durante as atualizações e criações evitará que o desenvolvedor faça outra chamada de API para obter a representação de recurso após a chamada de API.

## Mostrar relacionamento usando sub-recursos
Se você quiser mostrar relacionamentos em URIs, você pode fazer isso usando sub-recursos, mas você precisa garantir que isso seja feito corretamente e faça sentido para os usuários das APIs REST.

## Tratamento de erros RESTful e mensagens de resposta de status
Sua API deve fornecer mensagens de erro significativas e não simplesmente retornar o código de resposta de erro 400 Bad Request. 
Sua API deve retornar mensagens de erro úteis em um formato prescrito comum.

Um formato típico de mensagem de erro deve, no mínimo, retornar um código de erro e um campo de mensagem.

{
"status_code" : XXX,
"message" : "Oops, algo deu errado aqui"
}

Ou ainda mais detalhes:

{
"status_code" : XXX,
"message"  : "Oops, algo deu errado aqui",
"details" : "Forneça mais detalhes sobre as possíveis causas..."
}

## Usando códigos de status HTTP padrão
A API deve seguir a convenção de códigos de status HTTP padrão. Esses códigos de status de resposta HTTP são retornados sempre que visitantes do site ou mecanismos de busca fazem uma solicitação ao servidor web. Esses códigos numéricos de três dígitos indicam se uma solicitação específica foi bem-sucedida ou malsucedida.

### 1xx (Classe de Informação)
Esta classe de código de status é considerada experimental e não deve ser usada. Este código de status não requer cabeçalhos. O protocolo HTTP/1.0 não definiu nenhum código de status 1xx e, como tal, é altamente recomendado que os servidores NÃO enviem uma resposta 1xx.

### 2xx (Classe de Sucesso)
Esta classe de códigos de status indica que a solicitação do cliente foi recebida e processada com sucesso pelo servidor.

- 200 Ok – A solicitação foi bem-sucedida. As informações retornadas com a resposta dependem do método usado na solicitação.
- 201 Criado – A solicitação foi atendida e resultou na criação de um novo recurso. Este deve ser o código de status HTTP padrão usando o método POST quando um recurso é criado.
- 202 Aceito – A solicitação foi aceita para processamento, mas o processamento não foi concluído. A solicitação pode ou não ser eventualmente atendida. Neste caso, retornar uma mensagem de resposta JSON semelhante em formato à mensagem de erro pode ser útil, especialmente se associada a algum tipo de transaction_id. Este transaction_id pode ser usado posteriormente para pesquisar e garantir a conclusão adequada da solicitação.
- 204 No Content – ​​A solicitação foi atendida pelo servidor e não há conteúdo para enviar no corpo da resposta. Isso é normalmente usado com o método DELETE quando nenhum conteúdo precisa ser retornado. Também pode ser usado com o método PUT ao executar um UPDATE do recurso e as informações não precisam ser atualizadas.

### 3xx (Classe de Redirecionamento)
- 301 Movido Permanentemente – O recurso solicitado recebeu um novo URI permanente e quaisquer referências futuras a esse recurso DEVEM usar um dos URIs retornados.
- 304 Não Modificado – Este código de resposta indica que não há necessidade de retransmitir os recursos solicitados. É um redirecionamento implícito para um recurso em cache. Isso acontece quando o navegador armazena dados em cache, ele também armazena o cabeçalho Last-Modified ou ETag do servidor.

### 4xx (Classe de Erro do Cliente)
- 400 Solicitação Inválida – Este código de status de resposta indica que o servidor não conseguiu entender a solicitação devido a alguma sintaxe inválida. O cliente precisa modificar esta solicitação antes de retransmitir.
- 401 Não Autorizado – Este código de resposta de status de erro indica que a solicitação não foi processada porque não possui credenciais de autenticação válidas para o recurso de destino.
- 403 Proibido – Este código de resposta de status de erro indica que a solicitação não foi processada porque não possui credenciais de autenticação válidas para o recurso de destino. Se o servidor não desejar disponibilizar essas informações ao cliente, o código de status 404 (Não encontrado) pode ser usado. NOTA: Eu pessoalmente não concordo com essa abordagem, pois ela dará aos clientes uma indicação de que o endpoint RESTful não está mais disponível em vez de um problema de credenciais.
- 404 Não encontrado – Este código de resposta de erro indica que um servidor não consegue encontrar o recurso solicitado. Este código de resposta provavelmente é o mais famoso devido à sua frequência de ocorrência na web.
- 405 Método não permitido – O método de solicitação é conhecido pelo servidor, mas foi desabilitado e não pode ser usado. Por exemplo, podemos proibir o uso de DELETE em um recurso específico.
- 406 Não aceitável – Esta resposta é enviada quando o servidor web, após executar a negociação de conteúdo orientada pelo servidor, não encontra nenhum conteúdo seguindo os critérios fornecidos pelo agente do usuário. Podemos usar este código de resposta de status de erro para indicar que uma das condições ou parâmetros é
- 409 Conflito – Esta resposta é enviada quando uma solicitação entra em conflito com o estado atual do servidor.
- 412 Falha na pré-condição – O cliente indicou pré-condições em seus cabeçalhos que o servidor não atende

### 5xx (Classe de erro do servidor)
- 500 Erro interno do servidor – O servidor encontrou uma situação que não sabe como lidar.
- 503 Serviço indisponível – O servidor não está pronto para lidar com a solicitação. Causas comuns são um servidor que está inativo para manutenção ou que está sobrecarregado.

## Use SSL para segurança adicional – O tempo todo
No mundo de hoje, devemos usar SSL/TLS para todas as nossas conexões. No entanto, ainda é incrivelmente comum ver conexões não SSL (HTTP) em muitos lugares no cenário corporativo, bem como (bibliotecas, lojas, cafés, varejistas, etc.). 
Essas comunicações abertas permitem fácil espionagem e escuta e podem comprometer suas credenciais se você inadvertidamente se conectar e usar seus pontos de acesso Wi-Fi. 

Além de usar SSL para criptografia, devemos tomar as devidas precauções e executar o seguinte em nossa API:

- Proteja os métodos HTTP – A API RESTful geralmente usa POST, GET, PUT e DELETE para operações CRUD, ou seja, Criação, Leitura, Atualização e Exclusão. Precisamos garantir que o método HTTP de entrada seja válido para o token/chave de API e que o usuário da API associado tenha acesso à coleta, ação e registro de recursos.
- Proteja ações privilegiadas e coleções de recursos sensíveis – Novamente, nem todo usuário tem direito a todos os endpoints RESTful fornecidos pelo nosso serviço web. Isso é vital, pois você não quer que serviços web administrativos sejam mal utilizados.
Proteja contra falsificação de solicitação entre sites – Para recursos expostos por serviços web RESTful, é importante garantir que qualquer solicitação PUT, POST e DELETE esteja protegida contra falsificação de solicitação entre sites. Recomendamos usar uma abordagem baseada em token.
- Proteja-se contra referências diretas inseguras a objetos – Se você tivesse um serviço web REST de conta bancária, teria que garantir que houvesse uma verificação adequada das chaves primárias e estrangeiras:

É absurdo permitir algo assim, sem realizar validações extensivas:
/app-context/v2/account/87228723/transfer?amount=$10000.00&toAccount=2398239

- Realize validações de entrada – Devemos realizar a validação de entrada para todos os nossos aplicativos de interface de usuário e isso se aplica a serviços web RESTful, mas ainda mais porque ferramentas automatizadas podem facilmente martelar suas interfaces por horas a fio em velocidade extrema.