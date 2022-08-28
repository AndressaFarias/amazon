# TAG 

* Cada tag é um rótulo que consiste em uma chave e um valor definidos pelo usuário.

* As tags podem ajudár a gerenciar, identificar, organizar, pesquisar e filtrar recursos.

* Podem ser ciadas tags para categorizar recursos por propósito, proprietário, ambiente ou outros critérios.

* Cada tag possui duas partes:
    * Uma **chave** de marcação (por exemplo, `CostCenter`, `Environment`, ou `Project`).
        * As chaves de tag diferenciam maiúsculas de minúsculas.

    * Um **valor de tag** (por exemplo, `111122223333` ou `Production`).
        * Como as chaves de tag, os valores de tag diferenciam maiúsculas de minúsculas.

* As tasg pode ser adicionada, alterada ou removida um recurso de cada vez no console de serviço de cada recurso, service API, ou no AWS CLI.

# Melhores Práticas

* Use um formato padronizado com distinção entre maiúsculas e minúsculas para tags e aplique-o de forma consistente em todos os tipos de recursos.

* Use muitas tags em vez de poucas.

# Categorias de tagueamento

Uma "formula" eficaz no uso de tags é crir um agrupamentos de tags relevantes para o negócio para organizar os recursos nas dimensões técnica, comercial e de segurança. 

As empresas que usam processos automatizados para gerenciar sua infraestrutura também incluem tags adicionais específicas para automação.


# Limites e requisitos de nomenclatura de tag

* Cada recurso pode ter no máximo 50 tags criadas pelo usuário.

* Para cada recurso, cada chave de tag deve ser exclusiva e cada chave de tag pode ter apenas um valor.

* A chave da tag deve ter no mínimo 1 e no máximo 128 caracteres Unicode em UTF-8.

* O valor da tag deve ter no mínimo 0 e no máximo 256 caracteres Unicode em UTF-8.

    * Observação
    Alguns serviços não permitem tags com um valor vazio (comprimento 0).

*  Em geral, os caracteres permitidos nas tags são letras, números, espaços representáveis ​​em UTF-8 e os seguintes caracteres: _. : / = + - @. as deve ser consultada a documentação do serviço.

* Chaves e valores de tag diferenciam maiúsculas de minúsculas. Como prática recomendada, decida uma estratégia de declaração das chaves e valores de tag e implemente essa estratégia de maneira consistente em todos os tipos de recursos. 

    * Por exemplo, decida se usará `Costcenter`, `costcenter`ou `CostCenter` e use a mesma convenção para todas as tags.


# Estratégias de tagueamento comuns

## Tags para organização de recursos (Tags for resource organization)
As tags são uma boa maneira de organizar os recursos da AWS no AWS Management Console.

Pode ser configuradas tags para serem exibidas com recursos e pode ser feito pesquisas e filtro por tag.

Com o serviço AWS Resource Groups, você pode criar grupos de recursos da AWS com base em uma ou mais tags ou partes de tags. 


## Tags para alocação de custos (Tags for cost allocation)
O AWS Cost Explorer e os relatórios detalhados de faturamento permitem dividir os custos da AWS por tag.

Normalmente, são usadas tags de negócios, como *cost center/business unit, customer, ou project* para associar os custos da AWS às dimensões tradicionais de alocação de custos.


## Tags para automação (Tags for automation)
As tags de automação são usadas para ativar ou desativar tarefas automatizadas ou para identificar versões específicas de recursos para arquivar, atualizar ou excluir.


## Tags para controle de acesso (Tags for access control)
As políticas de IAM oferecem suporte a condições baseadas em tag, permitindo que você restrinja as permissões de IAM com base em tags ou valores de tag específicos.


# Governança de tagueamento
Uma estratégia de etiquetagem eficaz usa marcas padronizadas e as aplica de forma consistente e programática em todos os recursos da AWS. Você pode usar abordagens reativas e proativas para controlar as tags em seu ambiente AWS.


## A governança reativa 
serve para encontrar recursos que não estão marcados corretamente usando ferramentas como _Resource Groups Tagging API, AWS Config Rules_ e scripts personalizados. Para localizar recursos manualmente, você pode usar o _Tag Editor_ e relatórios de faturamento detalhados.


## A governança proativa 
Usa ferramentas como _AWS CloudFormation_, _AWS Service Catalog_, políticas de tag no _AWS Organizations_ ou permissões de nível de recurso do IAM para garantir que as tags padronizadas sejam aplicadas de forma consistente na criação do recurso.

Por exemplo, você pode usar _AWS CloudFormation `Resource Tags`  para aplicar tags a tipos de recursos. No _AWS Service Catalog_, você pode adicionar portfólio e tags de produtos que são combinados e aplicados a um produto automaticamente quando ele é iniciado. Formas mais rigorosas de governança proativa incluem tarefas automatizadas. Por exemplo, você pode usar a API de marcação de grupos de recursos para pesquisar as tags de um ambiente AWS ou executar scripts para colocar em quarentena ou excluir recursos marcados incorretamente.


# REFERENCIA
Tagging AWS resources
 :: https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html
