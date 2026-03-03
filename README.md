8# Bd_Roupa

## 📌 Apresentação do Projeto

O Bd_Roupa é um sistema simples para cadastro e organização de roupas, onde o usuário pode registrar suas peças, visualizar sua lista de itens e marcar quais deseja disponibilizar para venda ou troca.

O objetivo geral é permitir que o usuário organize seu guarda-roupa de forma digital e pratique o reaproveitamento de roupas de maneira simples e prática.

O público-alvo são pessoas que desejam organizar suas roupas e ter um controle básico sobre peças que desejam vender ou trocar.
---

## 👥 Equipe

- Adriano Moura
- Gabrielly Gonçalves  
- Luna Campolina
- Victor Eduardo

---
## 🚀 Versão Inicial

Versão 1.0

Funcionalidades previstas:

- Cadastro de usuários  
- Cadastro e listagem de roupas por usuário  
- Criação de pedidos de roupas entre usuários  
- Registro de pagamento associado ao pedido  
- Controle de entrega relacionada ao pedido  

---

## 📊 Modelo de Dados

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

    USUARIO ||--o{ ROUPA : oferece
    USUARIO ||--o{ PEDIDO : realiza
    ROUPA ||--o{ PEDIDO : é_solicitada
    PEDIDO ||--|{ PAGAMENTO : gera
    PEDIDO ||--o{ ENTREGA : tem
