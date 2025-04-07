# Mapeamento da Divulgação Científica - UFMG

Aplicativo Streamlit para visualização dos dados do mapeamento de divulgação científica da UFMG (2023-2024).

## Implementação em Produção

### Pré-requisitos
- Servidor Linux com Docker
- Acesso ao domínio `www.ufmg.br/proex/mapeamento-dc`
- Proxy reverso configurado

### Deploy com Docker

1. **Construir a imagem:**

```console
docker build -t ufmg/mapeamento-dc .
```

2. **Executar o container:**

```console
docker run -d
-p 8501:8501
-e STREAMLIT_SERVER_BASE_URL_PATH=/proex/mapeamento-dc
--restart unless-stopped
--name mapeamento-dc
ufmg/mapeamento-dc
```

### Configuração do Proxy (Exemplo Nginx)

```console
location /proex/mapeamento-dc {
proxy_pass http://localhost:8501/proex/mapeamento-dc;
proxy_set_header X-Forwarded-For scheme;
proxy_set_header Host $host;
}
```


### Variáveis de Ambiente

| Variável | Valor Padrão | Descrição |
|----------|--------------|-----------|
| STREAMLIT_SERVER_BASE_URL_PATH | /proex/mapeamento-dc | Caminho base da URL |
| STREAMLIT_SERVER_HEADLESS | true | Modo headless |

### Atualização
```console
docker stop mapeamento-dc
docker rm mapeamento-dc
docker pull ufmg/mapeamento-dc:latest
docker run -d [mesmos parâmetros acima]
```


## Documentação Técnica

### Estrutura do Projeto
.
mapeamento-dc/
├── .streamlit/ # Configurações do Streamlit
│ └── config.toml # Define porta, CORS e outras configurações
│
├── assets/ # Arquivos estáticos (imagens/ícones)
│
├── Dockerfile # Configuração para containerização
├── dashboard_dc.py # Aplicativo principal Streamlit
├── data_process.py # Script de processamento de dados
├── requirements.txt # Dependências Python (pandas, streamlit, etc.)
└── resultados_parciais.csv # Dataset principal em CSV


*Última atualização: 07/04/2025*