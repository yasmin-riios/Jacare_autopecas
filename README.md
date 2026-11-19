# Jacare_autopecas

Sistema de Gestão de Autopeças


Sistema web para centralizar vendas, estoque e consulta de peças de uma loja de autopeças, substituindo o controle manual em caderno e a dependência de catálogos externos.


Projeto acadêmico da disciplina Análise de Sistemas, do curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas da Faculdade Serra Dourada (Altamira-PA).

# Sobre o projeto:

A loja de autopeças controla vendas, estoque e identificação de peças de forma quase totalmente manual. A compatibilidade de uma peça com o modelo e o ano do veículo depende do conhecimento do vendedor e de consultas a catálogos online, e muitas peças são usadas e vêm de fornecedores de outros estados.

#	Dor	Efeito

1	Busca manual de peças	Demora no atendimento e dificuldade de localizar a peça fisicamente

2	Consulta a catálogos externos	Processo lento e risco de vender peça incompatível

3	Espera por cotação de fornecedor	Preço e frete definidos sem padronização

4	Vendas anotadas em caderno	Sem histórico de vendas e clientes, com erros de registro

5	Reposição por observação	Compra de peças sem base em dados

# Objetivo geral: 

centralizar em um sistema web as informações de vendas, estoque e consulta de peças, facilitando o trabalho do gerente e dos vendedores.

# Objetivos específicos:

Cadastrar e consultar peças com compatibilidade e localização

Controlar o estoque com alerta de mínimo

Registrar vendas e manter o histórico

Formar o preço com custo, frete e margem, bloqueando descontos abaixo da margem mínima

Acompanhar encomendas e trocas/devoluções

Gerar relatórios gerenciais com trilha de auditoria

Escopo

Dentro do escopo: UC01 a UC09 (usuários, peças, compatibilidade, estoque, preços, encomendas, vendas, trocas/devoluções e relatórios).

# Fora do escopo:

Emissão de nota fiscal (exigência fiscal ainda a validar)

Integração de software com maquininhas de cartão e Pix

Marketplaces

Aplicativo móvel para o cliente

# Perfis de usuário:

Perfil	Permissões

Gerente	Acesso total: usuários, cadastro de peças, preços e margens, estoque, relatórios e alertas

Vendedor	Consulta peças e estoque, negocia desconto (limitado pela margem), registra vendas, encomendas e trocas

Cliente	Não usa o sistema; é atendido pelo vendedor no balcão

# Requisitos:

Requisitos funcionais:

ID	Requisito	Descrição

RF01	Controle de usuários	Perfis de acesso restritos (gerente e vendedor), com trava de descontos baseada na margem de lucro

RF02	Cadastro de peças	Registro detalhado: especificações, compatibilidade, custos e localização na loja

RF03	Consulta de compatibilidade	Busca cruzada entre peças, modelos e anos dos veículos

RF04	Controle de estoque	Monitoramento de quantidades e alertas automáticos de estoque mínimo

RF05	Registro de vendas	Histórico da transação com itens, cliente, vendedor e dados de pagamento

RF06	Controle de encomendas	Rastreio de pedidos a fornecedores, gerenciando fretes e prazos

RF07	Formação de preços	Cálculo automático do valor de venda unindo custos, frete e margem

RF08	Trocas e devoluções	Fluxo reverso: motivos, estornos financeiros e devolução ao estoque

RF09	Relatórios	Visão gerencial de vendas, finanças, estoque e margens de lucro

Requisitos não funcionais:

ID	Requisito	Descrição

RNF01	Tecnologia	Aplicação web em nuvem, acessível por navegadores padrão, com dados centralizados

RNF02	Desempenho	Resposta de até 2 segundos nas operações principais

RNF03	Disponibilidade	Otimizada para desktop, com 99,5% de uptime

RNF04	Segurança	Autenticação segura e acesso aos dados restrito por perfil

RNF05	Usabilidade	Interface limpa e intuitiva, com treinamento rápido de novos funcionários

RNF06	Auditoria	Registro permanente de ações críticas (mudanças de preço, estornos etc.)

# Regras de negócio:

RN01: Preço de venda = custo + frete + margem de lucro

RN02: Desconto nunca pode reduzir o lucro abaixo da margem mínima definida pelo gerente

RN03: A reposição de estoque considera a média de vendas e a procura

# Casos de uso:

ID	Caso de uso	Atores	Requisitos

UC01	Gerenciar usuários	Gerente	RF01, RNF04

UC02	Cadastrar/atualizar peças (inclui entrada de estoque)	Gerente	RF02, RF04

UC03	Consultar peças/compatibilidade	Gerente, Vendedor	RF02, RF03

UC04	Consultar estoque	Gerente, Vendedor	RF04

UC05	Calcular preços/aplicar desconto	Gerente, Vendedor	RF07, RN01, RN02

UC06	Gerenciar encomendas	Gerente, Vendedor	RF06

UC07	Registrar venda	Vendedor	RF05, RF04

UC08	Registrar troca/devolução	Gerente, Vendedor	RF08

UC09	Consultar relatórios gerenciais	Gerente	RF09


Prioridade máxima (MVP): UC03, UC05 e UC07, considerados o "coração" da loja.

# Arquitetura:

Decisão (ADR-001): monolito modular em camadas (apresentação, regras de negócio e acesso a dados), com módulos separados por Blueprints do Flask.

Decisão (ADR-002): PostgreSQL como banco relacional, acessado via SQLAlchemy, com transações no registro de venda e na baixa de estoque.

Containers (C4 nível 2)

Container	Tecnologia	Responsabilidade

Interface Web	HTML, CSS, JavaScript	Telas e validações básicas

Proxy reverso	Nginx + Let's Encrypt	HTTPS, arquivos estáticos e repasse à aplicação

Aplicação back-end	Python 3, Flask, Gunicorn, SQLAlchemy	Regras de negócio, login, permissões, preço, estoque, relatórios e auditoria

Banco de dados	PostgreSQL	Dados do negócio e trilha de auditoria

Rotina de backup	cron + pg_dump	Cópia diária criptografada enviada a armazenamento externo (SFTP/HTTPS)

Por que monolito modular e não microserviços?

Loja com operação centralizada, equipe pequena e orçamento limitado: o monolito reduz o esforço de desenvolvimento e operação. A escolha pode ser reavaliada se alguma função precisar escalar de forma independente.

Segurança

Senhas com hash (bcrypt ou Argon2) e login individual

Permissões por perfil validadas no servidor

HTTPS e backup criptografado


Trilha de auditoria de preços, descontos e operações importantes
Coleta mínima de dados pessoais de clientes (nome e contato), conforme a LGPD (Lei nº 13.709/2018)

# Modelo de domínio:

Entidades do diagrama conceitual: Usuário, Venda, Item de Venda, Categoria, Peça, Movimentação de Estoque e Compatibilidade da peça com veículos.

# Viabilidade:

Todos os valores são hipóteses da equipe, não orçamentos contratados.

Dimensão	Conclusão

Técnica	Viável: tecnologia madura e carga muito abaixo da capacidade de uma VPS de entrada (2 vCPU / 4 GB)

Operacional	Viável, com carga inicial de dados, 2 sessões de treinamento (~3 h) e 2 semanas em paralelo com o caderno

Prazo	~20 semanas (~5 meses), 800 h de esforço, 40 h/semana

Econômica	Condicional: custo recorrente ~R$ 283/mês; desembolso mínimo em caixa ~R$ 880/ano; desenvolvimento (800 h × R$ 35/h = R$ 28.000) é custo de oportunidade

Premissas de dimensionamento: 1 gerente + até 4 vendedores (máx. 3 acessos simultâneos), até 5.000 peças, 30 a 60 vendas por dia, menos de 1 GB de dados no primeiro ano.

# Principais riscos:

Risco	Prob.	Impacto	Mitigação

Internet instável	Média	Alto	Procedimento manual de contingência e lançamento posterior

Falha no servidor/hospedagem	Média	Alto	Monitoramento e procedimento de recuperação

Perda de dados ou falha de backup	Baixa	Alto	Backup diário, cópia externa e teste de restauração

Erros no estoque	Média	Alto	Validações, registro de movimentações e conferências

Divergência em pagamentos	Média	Alto	Conferir comprovantes de Pix e maquininha

Emissão de nota fiscal	Média	Alto	Verificar exigências fiscais antes da implantação

Acesso indevido	Baixa	Médio	Login individual, senhas seguras e perfis

Informações incorretas de peças	Média	Médio	Confirmar com catálogos e fornecedores

# Cronograma estimado:

Etapa	Semanas	Esforço

1. Projeto detalhado e protótipos	1 a 3	120 h
   
2. MVP (acesso, peças, compatibilidade, estoque, preços e vendas)	4 a 11	320 h
   
3. Encomendas, trocas/devoluções e relatórios	12 a 15	160 h
   
4. Testes e homologação	16 a 17	80 h
   
5. Carga de dados, treinamento e implantação	18 a 20	120 h
    
# Equipe:

Felipe Deivid da Costa Saraiva

João Antônio Braga Portilho

Lucas Vinicius Morais Pereira

Yasmin Nicolly Carvalho Rios

