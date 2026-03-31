# 👗 Bd_Roupa - Sistema de Gestão e Troca de Roupas

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Status](https://img.shields.io/badge/Status-Fase%201%20Conclu%C3%ADda-success?style=for-the-badge)

## 📌 Apresentação do Projeto

O **Bd_Roupa** é um sistema projetado para o cadastro, organização e circulação de roupas. O ambiente permite que o usuário registre suas peças, visualize seu inventário digital e marque quais itens deseja disponibilizar para venda ou troca com outros usuários.

* **Tema Base:** Rede Social de Moda / E-commerce C2C.
* **Objetivo Geral:** Permitir que o usuário organize seu guarda-roupa de forma digital, facilitando o controle de peças e incentivando a moda circular e o reaproveitamento através de vendas e trocas de maneira simples e prática.
* **Público-Alvo:** Pessoas interessadas em organizar seu vestuário, praticar o desapego consciente e que desejam ter um controle básico sobre peças que querem vender ou trocar com outras pessoas.

---

## 👥 Equipe de Desenvolvimento

* Adriano Moura
* Gabrielly Gonçalves  
* Luna Campolina
* Victor Eduardo

---

## 🚀 Versão Inicial (v1.1) - Funcionalidades

- [x] Cadastro e autenticação de usuários.
- [x] Cadastro e listagem de roupas vinculadas a cada usuário.
- [x] Criação de pedidos de roupas entre usuários (compra/troca).
- [x] Registro de pagamento associado ao pedido.
- [x] Controle de entrega e status logístico relacionado ao pedido.
- [x] Gamificação em quiz implementada.
- [x] Protótipo de front-end iniciado. Link: https://stylequest.netlify.app/

---

## 📊 Modelo de Dados (Diagrama ER)

Abaixo está a representação do modelo de dados relacional construído para o sistema:

```mermaid
erDiagram
    USUARIO {
        int usua_id PK
        string usua_nome
        string usua_email
        string usua_senha
        string usua_telefone
        string usua_bio
        string usua_avatar_url
        int usua_xp
        int usua_nivel
        int usua_confianca_percent
        datetime usua_data_cadastro
    }

    ROUPA {
        int roup_id PK
        int roup_usua_id FK
        string roup_nome
        string roup_descricao
        string roup_categoria
        string roup_tamanho
        string roup_tempo_uso
        string roup_estado_conservacao
        decimal roup_preco "NULL se for apenas troca"
        enum roup_objetivo "venda | troca | ambos"
        string roup_imagem_url
        boolean roup_disponivel
    }

    PEDIDO_VENDA {
        int pedi_id PK
        int pedi_comprador_id FK
        int pedi_roupa_id FK
        datetime pedi_data_criacao
        enum pedi_status "pendente | pago | enviado | concluido | cancelado"
        decimal pedi_valor_total
    }

    PROPOSTA_TROCA {
        int troc_id PK
        int troc_solicitante_id FK
        int troc_proprietario_id FK
        int troc_item_ofertado_id FK
        int troc_item_solicitado_id FK
        datetime troc_data_criacao
        enum troc_status "pendente | aceita | recusada | finalizada"
    }

    CHAT_MENSAGEM {
        int mens_id PK
        int mens_troc_id FK
        int mens_remetente_id FK
        string mens_texto
        datetime mens_data_envio
    }

    PAGAMENTO {
        int paga_id PK
        int paga_pedi_id FK
        enum paga_status "pendente | confirmado | falhou"
        decimal paga_valor_total
        datetime paga_data_pagamento
        enum paga_metodo "cartao | pix | boleto"
    }

    ENTREGA {
        int entr_id PK
        int entr_pedi_id FK "Relaciona com Venda"
        int entr_troc_id FK "Relaciona com Troca"
        enum entr_tipo "propria | terceirizada"
        enum entr_status "pendente | em_rota | entregue"
        datetime entr_data_envio
        string entr_rastreio
    }

    USUARIO ||--o{ ROUPA : "cadastra"
    USUARIO ||--o{ PEDIDO_VENDA : "compra"
    USUARIO ||--o{ PROPOSTA_TROCA : "solicita ou recebe"
    ROUPA ||--o{ PEDIDO_VENDA : "eh vendida em"
    ROUPA ||--o{ PROPOSTA_TROCA : "faz parte de"
    PROPOSTA_TROCA ||--o{ CHAT_MENSAGEM : "possui"
    PEDIDO_VENDA ||--|| PAGAMENTO : "gera"
    PEDIDO_VENDA ||--o| ENTREGA : "gera"
    PROPOSTA_TROCA ||--o| ENTREGA : "gera"
