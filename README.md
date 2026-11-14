## Nova Vida Seguros: banco de dados

Modelagem em MySQL do banco de dados de uma operadora fictícia de seguros de vida, a Nova Vida Seguros. O banco organiza o cadastro de titulares, dependentes e contratos dos planos Prata, Ouro e Diamante, com carência e vigência, e foi pensado para dar suporte a uma aplicação web e à emissão futura de notas fiscais.

O projeto foi inspirado na minha experiência com cadastros, conferência de documentos e emissão de notas fiscais em rotinas administrativas. Todos os dados deste projeto são fictícios.

## Tabelas
- `dados_empresa`: Dados cadastrais e fiscais da empresa (CNPJ, inscrições, regime tributário, endereço), base para a emissão de NF
- `dados_usuarios`: Usuários do sistema, com perfil de acesso (DIREÇÃO, ADMINISTRAÇÃO, VENDAS, ATENDIMENTO) e status (ATIVO/INATIVO)
- `modelos_planos`: 	Modelos de plano (PRATA, OURO, DIAMANTE) com carência em dias e vigência em anos
- `cadastros`: Contrato de cada venda: plano escolhido, vendedor, usuário que registrou, datas, status do plano, números de proposta e recibo, controle de assinatura, termo sucessor e irregularidades de documentos
- `titulares`: 	Dados pessoais e de endereço do titular de cada cadastro
- `dependentes`: Dependentes vinculados a um cadastro, com documento e parentesco

## Regras de negócio
- Um titular por plano, e um plano por titular: id_cadastro e cpf são UNIQUE em titulares. Cada cadastro tem um único titular, e o mesmo CPF não pode ser titular de dois cadastros.
- Uma pessoa pode ser titular de um plano e dependente em outros: titulares e dependentes são tabelas separadas, sem restrição cruzada entre elas.
- Um dependente pode estar em mais de um plano, mas não duplicado no mesmo: UNIQUE (id_cadastro, tipo_documento, numero_documento) em dependentes.
- Vigência de 2 ou 4 anos: CHECK em modelos_plano.
- Um modelo por combinação plano/vigência: UNIQUE (nome_plano, vigencia_anos).
- CNPJ e CPF únicos, e valores de status, perfil, parentesco e tipo de documento restritos por ENUM.

## Regras de negócio definidas (ainda não aplicadas por restrição)
Carência: Prata 30 dias, Ouro 60 dias, Diamante 120 dias.
Diamante tem vigência apenas de 4 anos.
O titular é o responsável jurídico do contrato: responde por inclusões, exclusões e demais decisões sobre o plano.

## Como executar
Tenha o MySQL 8.0.16+ instalado.
Clone o repositório
Execute o script do schema. Ele está todo em um único arquivo, cria o banco nova_vida e todas as tabelas na ordem correta de dependência:

## Próximos passos
Criar o diagrama entidade-relacionamento (DER)
Adicionar script com dados fictícios para testes
Reforçar no banco as regras de carência e vigência do plano Diamante
Criação das tabelas de pagamento e cobrança
API REST em Java (Spring Boot)
Geração de nota fiscal simulada
