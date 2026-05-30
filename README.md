# Banco-de-Dados
Este repositório contém a infraestrutura "um banco de dados PostgreSQL com suporte a embeddings e busca semântica usando a extensão pgvector. É o ponto de partida para projetos que utilizam RAG com IA.

Aqui está uma versão reescrita do seu guia. O objetivo foi melhorar a escaneabilidade, organizar os blocos de código de forma mais limpa e dar um tom mais profissional e moderno ao README, mantendo toda a parte técnica intacta.

🚀 Primeiros Passos com pgvector e Docker
Este guia orienta a configuração rápida de um banco de dados PostgreSQL com suporte a busca vetorial utilizando a extensão pgvector.

📋 Pré-requisitos
Antes de começar, certifique-se de ter instalado em sua máquina:

Docker

Docker Compose

🛠️ Inicialização do Banco de Dados
Siga os passos abaixo para clonar o repositório, subir o container e validar a instalação.

1. Clonar o Repositório e Acessar a Pasta

git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio

2. Subir o Container
Execute o comando em segundo plano (-d) e aguarde 5 segundos para que o banco de dados inicialize completamente:

docker-compose up -d
sleep 5

3. Validar a Extensão pgvector
Verifique se a extensão foi instalada e ativada corretamente no banco de dados:

docker exec ia-vector-db psql -U admin -d vector_db -c "\dx"
💡 O que esperar: A extensão vector deve aparecer listada no retorno do terminal.

🔌 Conexão e Gerenciamento
Acessar o Banco de Dados (CLI)
Para interagir diretamente com o banco de dados via psql:

# Acessar o terminal interativo do PostgreSQL
docker exec -it ia-vector-db psql -U admin -d vector_db

# Dentro do psql, teste uma query de exemplo:
SELECT * FROM documents;
Encerrar o Serviço
Para parar os serviços sem perder os dados armazenados:

docker-compose down
⚠️ Atenção: Se desejar apagar completamente o banco de dados e deletar todos os dados salvos, use o comando abaixo (limpa os volumes):

docker-compose down -v

⚙️ Configurações do Ambiente

<img width="869" height="344" alt="image" src="https://github.com/user-attachments/assets/1cd41996-5be7-4ed9-9c2d-261c95f2b0d1" />

Estrutura do Projeto
docker-compose.yml: Configura o container utilizando a imagem oficial pgvector/pgvector:pg16 (PostgreSQL 16 pré-compilado com a extensão), define o mapeamento da porta 5432 e monta o volume pg_data para persistência.

init.sql: Script executado automaticamente apenas na primeira inicialização do container. Ele automatiza:

A criação da extensão pgvector.

A estrutura da tabela documents (com colunas de embeddings).

A criação de índices otimizados para buscas vetoriais.

📚 Próximos Passos
Integração: Conecte sua aplicação utilizando psycopg2 (Python), pg (Node.js) ou o ORM de sua preferência.

Embeddings: Gere os vetores usando APIs (OpenAI, Cohere) ou modelos locais (Hugging Face).

Implementação de RAG: Armazene os vetores gerados e configure buscas por similaridade semântica (cosseno, L2, produto escalar).

🐛 Resolução de Problemas (Troubleshooting)
Erro: porta 5432 já está em uso

Motivo: Você já tem um PostgreSQL nativo rodando na sua máquina.

Solução: Pare o serviço local ou mude a porta externa no docker-compose.yml para algo como 5433:5432.

Erro: extensão vector não encontrada

Motivo: Provavelmente o docker-compose.yml está usando a imagem padrão do postgres:latest.

Solução: Garanta que a imagem configurada seja pgvector/pgvector:pg16.

Erro: init.sql não foi executado

Motivo: O script só roda se o volume de dados estiver vazio. Se você já subiu o container antes de criar o init.sql, ele foi ignorado.

Solução: Resete o volume com docker-compose down -v e suba novamente com docker-compose up -d.

Erro: O container sobe, mas o banco recusa a conexão imediatamente

Solução: Dê tempo ao tempo. O PostgreSQL pode demorar alguns segundos para realizar os procedimentos de boot. Aguarde o sleep 5.

📖 Referências úteis
Repositório Oficial do pgvector

Documentação do PostgreSQL

Metodologia The Twelve-Factor App

☑️ Checklist de Validação
[ ] Docker e Docker Compose instalados e ativos.

[ ] Repositório clonado localmente.

[ ] Container iniciado com sucesso via docker-compose up -d.

[ ] Presença da extensão pgvector confirmada no banco.

[ ] Conexão externa disponível em localhost:5432.
