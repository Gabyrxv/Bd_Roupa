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

## 🚀 Versão Inicial (v1.0) - Funcionalidades

- [x] Cadastro e autenticação de usuários.
- [x] Cadastro e listagem de roupas vinculadas a cada usuário.
- [x] Criação de pedidos de roupas entre usuários (compra/troca).
- [x] Registro de pagamento associado ao pedido.
- [x] Controle de entrega e status logístico relacionado ao pedido.

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
        datetime usua_data_cadastro
    }

    ROUPA {
        int roup_id PK
        int roup_usua_id FK
        string roup_nome
        string roup_categoria
        string roup_tamanho
        decimal roup_preco
        boolean roup_disponivel
    }

    PEDIDO {
        int pedi_id PK
        int pedi_comprador_id FK "Usuário que faz o pedido"
        int pedi_roupa_id FK
        datetime pedi_data_criacao
        enum pedi_status "pendente | confirmado | cancelado | pago | enviado | concluido"
        decimal pedi_valor_total
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
        int entr_pedi_id FK
        enum entr_tipo_entrega "propria | terceirizada"
        enum entr_status_entrega "pendente | em_rota | entregue"
        datetime entr_data_envio
        datetime entr_data_entrega
        boolean entr_rota_calculada
        string entr_nota "Criar somente quando PAGAMENTO.paga_status = confirmado"
    }

    USUARIO ||--o{ ROUPA : "oferece"
    USUARIO ||--o{ PEDIDO : "realiza"
    ROUPA ||--o{ PEDIDO : "é solicitada em"
    PEDIDO ||--|{ PAGAMENTO : "gera"
    PEDIDO ||--o{ ENTREGA : "tem"
